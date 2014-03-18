==========================
Build Manager Installation
==========================

Users should begin by following the [common installation instructions] (Common-Install-Requirements) before proceeding to the buildmanager specific instructions below.

Third-party tools
-----------------

The buildmanager has several dependencies on third party libraries which must be installed and available in the system path

* Image Magick
  The 16 bits-per-pixel version of `Image Magick`_ should be installed and in the system path.  **Q16** should be in the Image Magick download filename which indicates it is a 16bpp build.  On Windows I use the shared library version (dll).

* ir-tools
  The original NCRToolset, a.k.a. "ir-tools" are located at `NCR Tools`_  For Windows a somewhat `optimized version`_ is available.  For Mac the version from `Brad Grimm`_ is required.

  The buildscripts were originally developed and used on Windows.  They should work on on other platforms but are not tested.  For Mac users I recommend using `Mac Ports`_ to install the numpy,scipy, and matplotlib libraries.

* Optional: win32api

  As of version 1.0.1 the build manager will attempt to lower its priority below that of a normal process on the machine.  This allows a machine to remain more interactive to users while doing builds.  This requires the pywin32 package. Obtainable from `Gohlke's site`_.

Installation
------------

There are many roads to installing the packages.  One of these options should work.  The alternatives are provided for troubleshooting 

1. Best option, use pip
   The simplest install is to go to the scripts directory for your python installation.  C:\Python27\scripts is a common location.  Install the nornir-buildmanager package directly from github via pip by typing the following command
   
   Pip has recently changed its behavior after version 1.4 to not install dependencies.  Using PIP 1.4 this command should be sufficient to install nornir.:: 
   
    pip install git+https://github.com/nornir/nornir-buildmanager.git --upgrade
    
   Later versions of Pip require each package to be installed seperately.::
    
    pip install git+https://github.com/nornir/nornir-shared.git --upgrade
    pip install git+https://github.com/nornir/nornir-pools.git --upgrade
    pip install git+https://github.com/nornir/nornir-imageregistration.git --upgrade
    pip install git+https://github.com/nornir/nornir-buildmanager.git --upgrade
 
  If you are on a machine that can build C extensions for Python packages, for example a working Mac Ports install, the above approach should work even if one hasn't manually installed the ore-requisite packages. 

2. Second option, use git and setup.py

   Open a command prompt "cmd.exe" and create a directory to store github code in.  I use _C:\src\git_.    
     1. Change to this directory::
     
          cd \src\git
        
     2. Download the source::
     
          git pull https://github.com/nornir/nornir-buildmanager.git  
       
     3. Install the package::
     
          python setup.py install

3. Manual Option:
   
   Download the `repository as a .zip file`_ .  If you want to change the source you should create a Github account, clone the repository, and work from that version.  Instructions for using git are beyond the scope of this installation manual.  A good starting point for learning about git is `Pro Git`_ by Scott Chacon. 
   
   Extract the source code to a directory on your local file system.  In this example I will use C:\src\git\nornir\nornir-buildmanager

   Open a console.  It is a good idea to start a new console to ensure all of the changes to the system path are active.  Change to the directory you extracted the nornir-buildmanager into and run the python setup.py install command, ex::
     C:\src\git\nornir\nornir-buildmanager>python setup.py install

Upgrade
-------

To be thorough I suggest uninstalling the previous nornir version using pip::

    pip uninstall nornir-buildmanager
    pip uninstall nornir-imageregistration
    pip uninstall nornir-pools
    pip uninstall nornir-shared
    
After uninstalling the old version follow the installation instructions above.  
 
If installation fails scan the output for evidence of a missing package that could not be installed.  The biggest cause of problems 
during an upgrade is nornir requiring an updated version of a library that PIP is unable to build locally.  For example Scipy, Numpy,
or Matplotlib.  This will cause the automated setup to fail.  In these cases the best approach is to locate the latest versions of
those packages, uninstall the old version, install the new version, and then attempt the installation again.

.. _Image Magick: http://www.imagemagick.org/
.. _Brad Grimm: http://sci.utah.edu/~bgrimm/nightly/
.. _NCR Tools: http://www.ucnia.org/download/ncrtoolset/
.. _optimized version: http://connectomes.utah.edu/Software/nornir/ir-tools_JA_Improved.zip
.. _Mac Ports: http://www.macports.org/
.. _Gohlke's site: http://www.lfd.uci.edu/~gohlke/pythonlibs/#pywin32
.. _repository as a .zip file: https://github.com/jamesra/nornir-buildmanager/archive/master.zip
.. _Pro Git: http://git-scm.com/book/ 
