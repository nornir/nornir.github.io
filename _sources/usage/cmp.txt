====================================
Overlaying TEM with Light Microscopy
====================================

One of the most powerful features of the Transmission Electron Microscopy (TEM) connectomics platform is the ability to interleave Light Microscopy (LM) sections into the TEM volume.


Collecting and Processing Data
------------------------------

Shading
_______

Light microscopy images can suffer from shading issues.  This results in a checkerboard pattern across the final mosaic.

It is best to configure the scope to eliminate shading.  If that is not possible there is a ShadeCorrect operation in Nornir.  ShadeCorrect builds a corrective image by measuring min/max values, depending on a brightfield or darkfield input, for each pixel position in the input images.  The corrective image can found in the filter directory containing the input images.  The corrective image is then added/subtracted from each image tile and a new filter is written for the channel.

The corrective image can introduce noise when every tile contains features or there are few tiles to sample from.  The problem is most easily corrected at capture time.  Collect 3-4 images from different background regions with no information to correctly determine the min/max values for the corrective image.  The tiles do not need to be contiguous with the main mosaic.  Using Nornir's prune operation should easily detect and discard these blank tiles once the shading correction has occurred.  It is advisable to remove these tiles with Prune even if they contiguous because blank tiles are very prone to registration  

Importing image tiles
_____________________

The Marc lab captures using Surveyor which stores tiles in a .pmg file format.  There is an "ImportPMG" operation which expects .pmg files to follow this naming convention:

.. code-block:: none
	
	<Slide#>_<Block#>_<Section#>_<Capture operator Initials>_<Magnification>_<Spot#>_<ChannelName>.pmg
	
	Slide#,Block#,Initials,Magnification,Spot# are used to preserve meta-data that may be specific to the Marc lab.  Dummy values can be inserted.
	
	The CMPBuild.cmd is a script in the standard installation which provides an example for building .pmg data.  The data should be aligned and assembled to a single image to be used as input to Pyre in the steps below.

Importing full images
_____________________

It is possible to process images outside of Nornir and then reimport them.  This is for cases where Nornir is missing functionality or is too cumbersome.
  
The ImportImages command expects images to follow this naming convention:

.. code-block:: none
	
	<Section#>_<Channel>_[Filter]_<Downsample>

For example, a file named 264_G_Enhanced_1.png would be added as the full-resolution image for the "Enhanced" filter of channel "G" in section 264.

The standard AssembleTiles command cannot be used for imported images.  Instead the "AssembleTilesFromImage" command should be used to generate optimized tiles directly from the imported image.


Selectively building Light Microscopy in a TEM volume
_____________________________________________________

Regardless of the source or imaging method each section cut should be assigned a distinct number assigned sequentially.  Among other benefits this practice allows Z depth calculations to be correct as the volume is constructed.

Importing Light Microscopy data is identical to importing TEM data.  Find the Nornir import operation for the data format you have captured in and import the data into the volume.  Ensure the channel specified in the filename is distinct from the channel used for TEM data.

We create volume-specific scripts for importing LM data into an existing TEM volume.  The key change is the use of the channels parameter to prevent operation on the wrong image channels.  For example to prevent our LM operations from affecting our darkfield or TEM data we apply the channel and regular expression:

.. code-block:: none

	-Channels "(?!(DAPI$)|(TEM$))"

The exclamation point tells Nornir not to process any channels named DAPI or TEM.  In the Marc Lab those names include all of the non-LM data.  We could also specifically name each LM channel name, but in our case we have far more LM channels than TEM or darkfield channels.

A reference scripts installed with Nornir is CMPIntoTEMBuild.cmd


Aligning Light Microscopy and TEM images
----------------------------------------

Automatic slice-to-slice registration will rarely succeed across channels.  The images can also be at vastly different scales. In this case manual registration is done with the Pyre tool.

The Marc Lab's procedure is to register full-resolution LM images to TEM images downsampled by a factor of 16.  We chose the highest resolution TEM image used for slice-to-slice TEM to TEM registration.  

The manual .stos files should be saved somewhere in the volume directory.  We then manually create a Stos group for these transforms and import them into the volume.  Here is a snippet of our script:

.. code-block:: none

	REM Be sure to adjust the downsample level to match the level used to align the CMP and TEM images.
	 
	title Create CMP Slice-to-slice alignment Group
	nornir-build Y:\Volumes\RC2 CreateStosGroup -StosGroup CMP -Downsample 16
	
	title TODO: Add .stos transforms to the Slice-to-slice alignment group
	REM Below is a template for .stos files.  Add one for each CMP section:
	
	nornir-build Y:\Volumes\RC2 AddStos -File Y:\Volumes\RC2\CMPManualStos\1405-1404_TomLec.stos -Block TEM -StosGroup CMP -ControlSection 1404 -ControlChannel TEM -ControlFilter Leveled -ControlDownsample 16 -MappedSection 1405 -MappedChannel TomLec -MappedFilter ShadingCorrected -MappedDownsample 1 -Type Grid
	nornir-build Y:\Volumes\RC2 AddToStosMap -Block TEM -Name FinalStosMap -ControlSection 1404 -MappedSection 1405
	
	title Merge CMP transforms into TEM transforms 
	nornir-build Y:\Volumes\RC2 CopyStosGroup -Input CMP -Output Grid -Downsample 16
	
	title SliceToVolume
	nornir-build %1 SliceToVolume -Downsample 16 -InputGroup Grid -OutputGroup SliceToVolume
	
	title ScaleVolumeTransforms
	nornir-build %1 ScaleVolumeTransforms -InputGroup SliceToVolume -InputDownsample 16 -OutputDownsample 1
	
	title CreateVikingXML
	nornir-build %1 CreateVikingXML -OutputFile SliceToVolume -StosGroup SliceToVolume1 -StosMap SliceToVolume
	
A breakdown of the script:

.. code-block:: none
	
	nornir-build Y:\Volumes\RC2 CreateStosGroup -StosGroup CMP -Downsample 16
	
CreateStosGroup is used to create a distinct slice-to-slice registration group that contains our LM to TEM registrations.  This is because they require seperate treatment than the automatically generated transforms.  The downsample parameter refers to the downsample level of the fixed images in the registration.

.. code-block:: none
	
	nornir-build Y:\Volumes\RC2 AddStos -File Y:\Volumes\RC2\CMPManualStos\1405-1404_TomLec.stos -Block TEM -StosGroup CMP -ControlSection 1404 -ControlChannel TEM -ControlFilter Leveled -ControlDownsample 16 -MappedSection 1405 -MappedChannel TomLec -MappedFilter ShadingCorrected -MappedDownsample 1 -Type Grid
	
AddStos adds a stos file to a stos group.  To be successful we must specify enough information to identify which section filters were used for registration.  This command places the .stos file in the CMP16 group and indicates which channel and filter should be used as control/mapped images.  Nornir changes the internal parameters of the .stos transform to make spatial units consistent.  The CMP16 stos group will not be used in builds.  It is used as a safe holding area to inject the transforms into the build process as needed.

.. code-block:: none 

	nornir-build Y:\Volumes\RC2 AddToStosMap -Block TEM -Name FinalStosMap -ControlSection 1404 -MappedSection 1405
	
This command updates the map used to define the relationships between all sections in the volume.  It indicates that there is a slice-to-slice transform that should be included in the volume.

The AddStos and AddToStosMap commands are repeated for each manual transform in the volume.  They can be executed repeatedly without harm.  The Marc Lab creates a single script which imports all LM to TEM stos files and appends it as needed.

.. code-block:: none 

	nornir-build Y:\Volumes\RC2 CopyStosGroup -Input CMP -Output Grid -Downsample 16
	
This command copies all of the .stos files we have imported to the CMP16 group to the Grid16 group.  The Grid16 group is used by our primary build script to generate all slice-to-volume transforms. 

.. code-block:: none
  
	nornir-build %1 SliceToVolume -Downsample 16 -InputGroup Grid -OutputGroup SliceToVolume
	
	nornir-build %1 ScaleVolumeTransforms -InputGroup SliceToVolume -InputDownsample 16 -OutputDownsample 1
	
	nornir-build %1 CreateVikingXML -OutputFile SliceToVolume -StosGroup SliceToVolume1 -StosMap SliceToVolume

These commands are replicated from our primary TEMAlign.cmd script.  The SliceToVolume line creates a transform that registers each section in the volume to a single center section to create a slice-to-volume transform.  The ScaleVolumeTransforms command scales the transforms to full-resolution.  The CreateVikingXML generates the meta-data required for Viking to view the volume.
	
About.xml
---------

Once the LM sections are in the volume it is desirable to overlay the LM data over TEM data.  This can be done automatically in Viking by updating the volume's about.xml with a channel mapping.

.. code-block:: xml
	
	<Section Number="124">
		<ChannelInfo>
			<Channel Channel="Selected" Color="0xFFFFFF" Section="Selected"/>
			<Channel Channel="InvertedLeveledShadingCorrected" Color="0x0000FF" Section="123"/>
		</ChannelInfo>
	</Section>
	
The above snippet places section #123 into #124's blue channel.

Once CreateVikingXML is run on the volume the about.xml entries will be copied into the .VikingXML file and the channel will appear in Viking.

If one does not want to run CreateVikingXML the identical snippet can be appended to the matching <Section> element in the .VikingXML file.



