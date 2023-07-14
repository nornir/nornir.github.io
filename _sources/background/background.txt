----------
Background
----------

Glossary of terms in context
============================

To build a 3D volume using our transmission electron microscopes we begin by taking a sample and embedding it in a **block** of plastic resin.  This block is then cut into thin, about 70-90nm, **sections**.  Each section is placed on a grid or a slide.  At 5000X our transmission electron microscope can produce an image representing an 8um square.  To collect a larger area we collect a **mosaic** of images with a 10%-12% overlap.  We refer to an image in a mosaic as a **tile** even though proper tiles wouldn't overlap.  Nornir figures out how to layout the tiles so we can **assemble** a single image of the entire mosaic.

We assemble a mosaic image for each section cut from the block.  Sometimes we call a section a **slice**.  Each section is then registered to the adjacent sections to produce **slice-to-slice** (stos) transformations.  We decide which mosaic is the **center** of the volume.  Transforms can then be added such that any point on a mosaic can be mapped through stos transforms until the center is reached.  This produces a mapping for each section to the volume. The **slice-to-volume** transforms can be used with Viking_ or if the mosaic images are small enough volume aligned section images can be produced.

.. _Viking: http://connectomes.utah.edu/http://marketplace.eclipse.org/marketplace-client-intro?mpc_install=61063