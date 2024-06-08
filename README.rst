###
dm4
###

A simple pure Python file reader for Digital Micrograph's DM4 file format.

This package would not have been possible without the documentation provided by Dr Chris Boothroyd at `<https://personal.ntu.edu.sg/cbb/info/dmformat/index.html>`_. Thank you!

############
Installation
############

Install using pip from the command line::

   pip install dm4

#######
Example
#######

Create a virtual environment::

   python -m venv .venv

Activate the virtual environment::

   source .venv/bin/activate

Install the dm4 package along with dependencies for the example::

   pip install dm4[example]

Create a Python script, example_dm4.py, with the following content::

   import dm4
   import numpy as np

   from PIL import Image

   with dm4.DM4File.open("replace-me.dm4") as dm4data:
      tags = dm4data.read_directory()

      image_data_tag = (
         tags.named_subdirs["ImageList"].unnamed_subdirs[1].named_subdirs["ImageData"]
      )
      image_tag = image_data_tag.named_tags["Data"]

      XDim = dm4data.read_tag_data(
         image_data_tag.named_subdirs["Dimensions"].unnamed_tags[0]
      )
      YDim = dm4data.read_tag_data(
         image_data_tag.named_subdirs["Dimensions"].unnamed_tags[1]
      )

      np_array = np.array(dm4data.read_tag_data(image_tag), dtype=np.uint16)
      np_array = np.reshape(np_array, (YDim, XDim))

      output_fullpath = "example.tif"
      image = Image.fromarray(np_array, "I;16")
      image.save(output_fullpath)

Run the script::

   python example_dm4.py

####
Todo
####

Reading arrays of groups has not been implemented.
