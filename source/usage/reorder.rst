Reordering Sections in a Volume
-------------------------------

    Occasionally section numbers may be reported in error.  When this occurs there are two broad strategies to repairing the mistake.  The most reliable and computationally expensive approach is:
    
    * Delete the mis-numbered section folders from the file system
    * Rename the folders containing the raw data with the correct section numbers
    * Reimport the affected sections and rebuild
    
    The downside to the above strategy is it loses any existing annotations for those sections placed in Viking and also requires repeating quite a bit of computation to re-register the sections.  The alternative approach is more labor intensive but is usually shorter overall:
    
    * Always rename the raw data with the correct section numbers.  While purely precautionary if the volume were rebuilt from scratch this will prevent a recurrence of the mistake in the future.
    * Rename the section folders within the volume folder using the correct section numbers.
    * Open the VolumeData.xml meta-data files within the affected section folders.  Update the attributes of the <Section> element to reflect the corrected section number, typically:
    
        * Number
        * Path
        * Name
    
    * Delete/Rename files that have the old section number.  Typically:
    
        * Assembled images, for example:
            
            * 'Images' folders under a filter
            * 'Mask' filters
            * 'Blob' filters
        
        * Optinal: For SerialEM data the .idoc, .log, .txt, .json meta-data imported from the microscope into the TEM channel folder under the section folder often reflect the original incorrect section number. It is reasonable to update these file names, however if this is done the VolumeData.xml file in the Channel folder must be opened.  The "Path" attribute of the corresponding <Data> elements must be updated to refer to the renamed files.
        
    * Viking Users Only: If you are annotating the Nornir volume with the Viking tools and there are annotations on the section it is possible to save those annotations if there are two adjacent sections whose position in the volume is simply being swapped.  A script for this can be found in Viking's git repository at Viking\Servers\SQL\UtilityScripts\SwapLocationsOnSections_V2.sql 
    
    * Update any external records you may keep outside of the Nornir environment regarding section numbers and renaming.
    
    * The final step is to regerate any data we removed during the renaming operation:
    
        * Run Assemble for any filters with images that were deleted
        * Run slice-to-slice registration to incorporate the newly ordered sections into the volume correctly
        * Run any reports that need to reflect the correct ordering
    
Reordering Example
------------------

    This is a step-by-step reordering of two adjacent sections of a TEM volume captured via SerialEM.  The volume name is RPC2 and the sections whose positions we want to swap are 291 & 292.  The volume lives in the W:\Volumes\RPC2 directory
    
    For utility, I created two cmd scripts:
    
    swap_names.cmd - Swaps the names of two folders::
    
        move %1 %1_original
        move %2 %1
        move %1_original %2
    
    CleanReorderedSection.cmd - Deletes folders that refer to the old section number::
    
        rmdir /s /q %1\TEM\%2\TEM\Leveled\Images
        rmdir /s /q %1\TEM\%2\TEM\Mask
        rmdir /s /q %1\TEM\%2\TEM\Blob
    
    Using the command line::
        
        > W:
        > cd \Volumes\RPC2\TEM            #Change directory to the folder containing the volume's sections
        > swap_names 0291 0292            #Renames the section folders

            W:\Volumes\RPC2\TEM>move 0291 0291_original
            1 dir(s) moved.
            W:\Volumes\RPC2\TEM>move 0292 0291
            1 dir(s) moved.
            W:\Volumes\RPC2\TEM>move 0291_original 0292
            1 dir(s) moved.

        > CleanReorderedSection W:\Volumes\RPC2 0291     #Remove files with the old section number
        
            W:\Volumes\RPC2\TEM>rmdir /s /q W:\Volumes\RPC2\TEM\0291\TEM\Leveled\Images

            W:\Volumes\RPC2\TEM>rmdir /s /q W:\Volumes\RPC2\TEM\0291\TEM\Mask
    
            W:\Volumes\RPC2\TEM>rmdir /s /q W:\Volumes\RPC2\TEM\0291\TEM\Blob
            The system cannot find the file specified.                          #This error is fine.  This volume doesn't use blob images or they weren't assembled yet.
            
        > CleanReorderedSection W:\Volumes\RPC2 0292
        
            W:\Volumes\RPC2\TEM>rmdir /s /q W:\Volumes\RPC2\TEM\0292\TEM\Leveled\Images

            W:\Volumes\RPC2\TEM>rmdir /s /q W:\Volumes\RPC2\TEM\0292\TEM\Mask
    
            W:\Volumes\RPC2\TEM>rmdir /s /q W:\Volumes\RPC2\TEM\0292\TEM\Blob
            The system cannot find the file specified.                          #This error is fine.  This volume doesn't use blob images or they weren't assembled yet.
    
    Update the meta-data
    
        1. Open W:\Volumes\RPC2\TEM\0291\VolumeData.xml I typically use Notepad++. If the XML Tools plugin is installed in Notepad++ one can format the VolumeData.xml using the CTRL+ALT+SHIFT+B shortcut.
        2. This is the existing meta-data::
        
            <Section CreationDate="2020-05-22 10:23:42" Name="0292" Number="292" Path="0292" Version="1.0">
                <Channel_Link CreationDate="2020-05-22 10:23:50" Name="Registered_TEM" Path="Registered_TEM" Version="1.0" />
                <Channel_Link CreationDate="2020-05-22 10:23:50" Name="TEM" Path="TEM" Version="1.0" />
            </Section>
            
        3. Update the <Section> element to change the Name, Number, and Path attributes::
        
            <Section CreationDate="2020-05-22 10:23:42" Name="0291" Number="291" Path="0291" Version="1.0">
                <Channel_Link CreationDate="2020-05-22 10:23:50" Name="Registered_TEM" Path="Registered_TEM" Version="1.0" />
                <Channel_Link CreationDate="2020-05-22 10:23:50" Name="TEM" Path="TEM" Version="1.0" />
            </Section>
            
        4. Repeat the process for W:\Volumes\RPC2\TEM\0292\VolumeData.xml, but setting values to 292 instead of 291
        
        5. (Alternatively to the steps above one could update the swap_names.cmd to also swap the meta-data files.  However both sections must have perfectly matching child elements for that to work.)
        
    With the meta-data updated the build can be run::
        
        RPC2_Build.cmd W:\Volumes\RPC2   #The volume specific command to assemble a mosaic and assemble images
        RPC2_Align.cmd W:\Volumes\RPC2   #The volume specific command to update slice-to-slice registration and reports
        
    Since RPC2 has annotations the SQL server must be updated.  Since these sections are adjacent we can rescue the annotations.  SQL server can be updated while the nornir build is running as they are not connected:  
        
        1. From the Viking source on github open Viking\Servers\SQL\UtilityScripts\SwapLocationsOnSections_V2.sql in SQL Server Management Studio.
        2. Ensure the connection the query is using is set to the correct volume, in the case of this example RPC2.
        3. Update the first two variables to match the section numbers being swapped::
    
            /* A function to swap annotations on two adjacent sections while preserving links*/
            declare @ZA int 
            declare @ZB int 
            
            set @ZA = 291
            set @ZB = 292
            ...
        4. Optionally, backup the database if you are familiar with how to do this.  If not backups are automatically performed nightly.
        5. Run the query, wait until it completes successfully.
        
    After the nornir build is done we can optionally refresh the VolumeShape column.  That column value is cached and needs to be refreshed using the updated slice-to-slice transform.  That can be done with the VikingAU tool.  Documentation for that tool will live in Viking's documentation when it is completed.        
    
    
    
        