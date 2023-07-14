===============
Troubleshooting
===============


Installation
------------

* **Q:** "python setup.py install" results in **unknown url type: git+http** being displayed and the setup script exits

  **A:** This indicates that [PIP](http://www.pip-installer.org/en/latest/installing.html) is not installed.
    
* **Q:** I get "Access denied" trying to follow the installation instructions

  **A:** Make sure you are running the command prompt as an administrator.  Right clicking the command prompt icon in windows and select "Run as admistrator".  One can also adjust an advanced property to always run the prompt as administrator.

* **Q:** Upgrading with `pip install git+https://github.com/jamesra/nornir-buildmanager.git --upgrade` did not succeed.  It looks like it is trying to install a package?

  **A1:** One known cause is earlier versions of packages seem to override later versions of packages according to pip.  On a system with Numpy 1.7.1 and 1.8 installed pip will only see 1.7.1 and attempt to download and compile numpy 1.8.  Verify the numpy version pip sees by looking at the known version for each installed package using::
  
             pip list
         
  The solution is to delete any versions of packages older than the latest version.  Pip can do this by entering::
         
             pip uninstall numpy
         
  Verify the correct version is detected by re-running "pip list".  Repeat the uninstall command until the most recently installed version is listed, or until nothing is listed and a clean install can be performed.
  
* **Q:** PIP says something about dependency_links requiring extra flags?
  
  **A:** This began with PIP 1.5.  The current workaround is to install each package separately as detailed on the installation page. 


Pyre
----

* **Q:** .stos file will not load, file cannot be found
 
  **A:** The .stos format stores the full path to the images as the first two lines of the file.  Open the .stos file in a text editor and ensure the paths to the files are correct.  Alternatively, find the images within the volume directory and drag-drop them into Pyre directly.
  
Eclipse
-------

* **Q:** The cluster pool tests fail with "Communication pipe read error"
  **A:** Make sure Eclipse is not connecting to subprocesses.  This option is toggled in the Pydev "Run/Debug" preferences.
  
* **Q:** The unit tests take forever to run"
  **A:** Twenty minutes is fairly typical for unit tests on a beefy machine.  Most should run in less than a minute.  The longest tests are IDoc and PMG tests.  Again, make sure Eclipse is not connecting to subprocesses.  This option is toggled in the Pydev "Run/Debug" preferences.
   