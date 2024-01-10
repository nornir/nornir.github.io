0. Starting in SQL Server Management Studio

1. Check the last modified date to determine if a backup is needed:
	``'select TOP 5 ID, LastModified from Location order by  LastModified DESC'

2. If annotations have occurred in the last 24 hours add a manual backup.  
  a. Right click Database context menu, choose tasks->Backup
  b. Ensure correct database is chosen from dropdown menu
  c. Select "OK", it should default to replacing the backup from the night before.
  d. Wait for success message before continuing.
  
3. Open Viking\Servers\SQL\UtilityScripts\SwapLocationsOnSections_V2.sql
	a. Update ZA and ZB with section numbers you want to swap.
	b. Ensure correct database is selected
	c. Run the query
	
From Windows explorer
	a. Rename the section folders to the correct numbers
	b. Open VolumeData.xml in Notepad++ (CTRL+ALT+SHIFT+B to format XML)
	c. Renumber the name, number, and path attributes to reflect the correct section number for both sections.
	d. From explorer, delete the assembled images.  This includes the <Volume/<Section#>/TEM/Leveled/Images and <Volume/TEM/<Section#>/TEM/Mask/Images.
	
From command prompt:
	a. To be extra safe, create a copy of the meta-data:
		nornir-build <VolumeDir> ExportMetadata
	   That helps with a recovery if a disaster happens
	b. Create backup copies of RPC2/TEM/Grid8 and RPC2/TEM/Volume1 or whatever the top level transform.
	c. Run the build command to regenerate assembled images (or use a more targeted build command for the purpose of swapping sections to save time.)
	d. Run the align command for the volume you have changed.
	
Check results in Viking when align commmand is completed.

  