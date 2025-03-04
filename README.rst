###
dm4
###

A pure python file reader for Digital Micrograph's DM4 file format

This package would not have been possible without the documentation provided by Dr Chris Boothroyd at http://www.er-c.org/cbb/info/dmformat/ Thank you.

############
Installation
############

Install using pip from the command line::

   pip install dm4

#######
Example
#######
   
Below is a short example of reading the image data from a dm4 file.  A more complete example can be found in the tests.::

   import dm4
   import PIL

   input_path = "your_filename_here.dm4"

   with dm4.DM4File.open(input_path) as dm4file:
       tags = dm4file.read_directory()

       image_data_tag = tags.named_subdirs['ImageList'].unnamed_subdirs[1].named_subdirs['ImageData']
       image_tag = image_data_tag.named_tags['Data']

       XDim = dm4file.read_tag_data(image_data_tag.named_subdirs['Dimensions'].unnamed_tags[0])
       YDim = dm4file.read_tag_data(image_data_tag.named_subdirs['Dimensions'].unnamed_tags[1])

       image_array = np.array(dm4file.read_tag_data(image_tag), dtype=np.uint16)
       image_array = np.reshape(image_array, (YDim, XDim))

       output_fullpath = "sample.tif"
       image = PIL.Image.fromarray(image_array, 'I;16')
       image.save(output_fullpath)

############
Script usage
############

The dm4 module can be invoked as a script to print a dm4 file's full directory to the command line.  This is helpful when determining the exact structure of a specific DM4 file: ::

    python -m dm4 your_dm4_file.dm4

################
Helper Functions
################

Print all of the tags and directories contained in a dm4 file to the console::

  dm4.print_tag_directory_tree(dmfile: DM4File, dir_obj: DM4TagDir, indent_level: int = 0)

Print data associated with a specific tag to the console, if it is printable::

  dm4.print_tag_data(dmfile: DM4File, tag: Union[DM4TagHeader, DM4DirHeader], indent_level: int)


####
Todo
####

Reading arrays of groups has not been implemented.
