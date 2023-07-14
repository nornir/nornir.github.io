Nornir has been a long project assembled by a single developer.  As such there are always blindspots.

Missing features
----------------

These features make sense but are not required for the immediate needs of any users.  An E-mail request is likely to bump the priority of these features.

Multiple channels
=================

1. Generating registrations from multiple channels from a single section directly into the volume.  
   
   This feature was working at one point but was dropped to get the v1.1 release done.  
   
2. A mechanism to register multiple channels from a single section to each other. 

   This need arises with captures of multiple fluorophores or when aligning optical and electron microscopy images.  There is no definined mechanism to store these registrations in the volume's meta-data.  The workaround is to create a fake section number for each channel.
   
Import issues
=============

1. Import a single image for a section. 
 
   It is easier to import a single large image for a section in a volume rather than a set of tiles.  This has worked in the past.  
   However the importer and exporter do not have beginning to end integration tests written.  If one wants to export registered images
   from this type of input the SliceToVolume pipeline should be used.  Send an E-mail if issues are encountered so I can add this
   workflow to the tests. 
   
2. Overlapping mosaic tiles without position meta-data. 
 
   On the todo list is to take a directory of overlapping images and produce the best guess mosaic.  Initial work has
   been done but it is not accessible yet.  The runtime of scales exponentially with the number of tiles.
          
Internal quirks
===============

1. The volume metadata is currently stored in tree of plain text XML files.  The XML is read and converted into objects at runtime.  This decision is very high in the list of design decisions I would like to revisit.  Reading and writing the XML scales poorly and the search functionality is not adequate.  If could do the project over again I would use a real database or ZODB.
 
Future work
-----------
     
1. Web server for volume images

   I'm planning to provide a package which handles HTTP queries and returns images registered into the volume. 
   
2. Pyre documentation

   The method for manual registration fixes is undocumented.  This should change soon.
   
3. User interface
 
   It is too difficult for users to adjust volume variables such as the contrast parameters for a channel.  Pipelines will be added to allow this operation without editting the XML files directly.
   
   The time to author a GUI is not current available.   
   
 

   