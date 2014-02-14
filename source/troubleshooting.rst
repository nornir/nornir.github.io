===============
Troubleshooting
===============


Installation
------------

* **Q:** "python setup.py install" results in **unknown url type: git+http** being displayed and the setup script exits

  **A:** This indicates that [PIP](http://www.pip-installer.org/en/latest/installing.html) is not installed.

* **Q:** Upgrading with `pip install git+https://github.com/jamesra/nornir-buildmanager.git --upgrade` did not succeed.  It looks like it is trying to install a package?

  **A:** One known cause is earlier versions of packages seem to override later versions of packages according to pip.  On a system with Numpy 1.7.1 and 1.8 installed pip will only see 1.7.1 and attempt to download and compile numpy 1.8.  Verify the numpy version pip sees by looking at the known version for each installed package using::
  
             pip list
         
  The solution is to delete any versions of packages older than the latest version.  Pip can do this by entering::
         
             pip uninstall numpy
         
  Verify the correct version is detected by re-running "pip list".

* **Q:** I get "Access denied" trying to follow the installation instructions

  **A:** Make sure you are running the command prompt as an administrator.  Right clicking the command prompt icon in windows and select "Run as admistrator".  One can also adjust an advanced property to always run the prompt as administrator.



