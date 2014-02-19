.. Nornir documentation master file, created by
   sphinx-quickstart on Wed Dec 18 12:02:16 2013.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.  
  
Welcome to Nornir's documentation!
==================================

The repository for Nornir lives at `http://github.com/nornir/`_

Contents
--------

.. toctree::
   :maxdepth: 2
   
   Overview <overview/overview>
   Installation <installation/toctree>
   Use <usage/toctree>
   Packages <packages/packages>
   Known Issues and Future Work <futurework>
   TroubleShooting <troubleshooting>
   Glossary and Background <background/background>
   
About Nornir
------------

Nornir's mission is to take large sets of overlapping images in 2D and 3D and produce registered (a.k.a. aligned) 3D volumes of any size and scale.  

History
-------

Nornir evolved from a collaboration between the `Marc Lab`_ and the `Scientific Computing Institute's`_ Tasdizen and Whitaker groups at the University of Utah.  The original tools, known as the `NCR Toolset`_ were used to construct `RC1`_, a 250um diameter, 33um tall cylinder of rabbit retina at a resoultion of 2.18 nm/pixel.   

Nornir is a work in progress.  It currently supports importing images from transmission electron microscopes running SerialEM (.idoc files), light microscopes running Surveyor (.pmg files), and other volumes represented with a single image per section (.png files).

Related Tools
-------------

Volume images frequently exceed the reasonable sizes for single images.  `Viking`_ is another Marc lab tool for viewing and annotating huge datasets.  Nornir produces volumes that can be viewed using Viking if placed on a web server.

Analysis of the annotations created by Viking for our existing rabbit connectome and future connectomes is performed by the `py-connectome-analysis`_ package 

Indices and tables
==================
 
* :ref:`genindex`
* :ref:`modindex`
* :ref:`search`

.. _http://github.com/nornir/: http://github.com/nornir/
.. _rc1: http://www.ncbi.nlm.nih.gov/pubmed/21311605/
.. _marc lab: http://prometheus.med.utah.edu/~marclab/marclab_09_science-papers.html/
.. _ncr toolset: http://www.ucnia.org/download/ncrtoolset/
.. _scientific computing institute's: http://www.ucnia.org/
.. _Viking: http://connectomes.utah.edu/
.. _py-connectome-analysis: https://github.com/jamesra/py-connectome-analysis/wiki
  