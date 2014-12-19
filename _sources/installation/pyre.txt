=================
Pyre Installation
=================

Users should begin by following the :ref:`common installation instructions <common>` before proceeding to the Pyre's specific instructions below.

Python packages
---------------

Use the latest 64-bit version of packages unless otherwise specified

* `WxPython 2.8 unicode or later`_
* `Pyglet 1.2alpha1 or later`_ The best method of installing Pyglet is calling pip from Python install scripts directory, which is typically C:\\Python27\\scripts::
  
      pip install --upgrade http://pyglet.googlecode.com/archive/tip.zip
    
* `PyOpenGL 3.X`_ can be installed from Gohlke's site.  `PyOpenGL Accelerate 3.X`_ is optional, but may increase performance.

Installing Pyre package
-----------------------

From the Python scripts directory use pip to install pyre directly from github::

    pip install git+https://github.com/nornir/nornir-pyre.git

For reasons uninvestigated Pip can fail to install other nornir packages.  If this occurs one can create an empty directory, clone the repository into it, and use python setup::
 
    git clone https://github.com/nornir/nornir-pyre.git
    python setup.py install
    
Pyre on the Mac
---------------

X11 is required and is available from 'XQuartz'_

Users have had trouble getting Pyre running on the Mac.  Some have no trouble and others spend a lot of time fighting the wxPython library.  The problem is not well investigated so if a solution is found please E-mail so we can document.

Launching Pyre
--------------

Once installed pyre creates a python script for launching pyre and well as some executable shortcuts.  The current practice is to launch from the Python scripts directory::

    python start_pyre.py

If that fails download the source to a directory using `git clone https://github.com/jamesra/nornir-pyre.git` command, change to the directory that command creates, and then run start pyre::
  
    python scripts/start_pyre.py
    
.. _WxPython 2.8 unicode or later: http://www.wxpython.org/download.php#stable
.. _Pyglet 1.2alpha1 or later: http://www.pyglet.org/download.html
.. _PyOpenGL 3.X: http://www.lfd.uci.edu/~gohlke/pythonlibs/#pyopengl
.. _PyOpenGL Accelerate 3.X: http://www.lfd.uci.edu/~gohlke/pythonlibs/#pyopengl-accelerate
.. _XQuartz: http://xquartz.macosforge.org/trac