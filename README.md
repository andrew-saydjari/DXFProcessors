# DXFProcessors

## DXF Preprocessing
DXF Preprocessing for CAD used to prepare electron-beam lithography files. Two seperate functionalities are included.
Both functionalities have in notebook plotting of the final DXFs for quality assurance. 
Checking the CAD in an external program is recommend before finally using for lithography.

### DXF Chip Builder
This code allows fast definition of a matrix of devices of the same size from individual DXF files. 
The user specifies which devices go where, and the number of rows and columns of devices.
Label handling and alignment mark placement are also implemented.
A template can be used instead of the alignment marks/labeling where the script just drops in the devices in a row/column format of a user made DXF.
Note that unlike the INSERT object (aka MATRIX) in DXF which frustrates some lithography tools and can only replicate the same device,
here the user can specify any device at any position, and can keep a copy of the generating script as a record.
For integration with ElionixBatchExport Script, the user should surround each device with a bounding box on its own layer (usually called "matrix")

### Boolean Builder
Most CAD programs that support DXF, do not support Boolean operations for the polyline class (which is used in all of lithography). 
All of the CAD programs that do Boolean operations for such objects, do not play well with DXF and create an import/export nightmare.
This function is simple. With a simple python enviornment, you can take a DXF file, do any Boolean operation on any two layers,
and get a DXF with the output of that Boolean operation as a new layer (in a DXF named "outputFilename_c.dxf")
Support for multiple layer Boolean operations is planned, but not currently implemented.

## ElionixBatchExportScript

Every part of this code works on LWPOLYINES and LWPOLYLINES ONLY.
If you are doing lithography, you should be using nothing else.
If you have circles, you should explode them to polylines with fixed resolution, because this is what lithography softwares read... etc.
All alignment marks are indicated by the bottom left corner of rectangles on the alignment layers
The matrix layer should outline the extent of each device (a bounding box). This allows the software to find a bottom left corner to be the origin.
This code can take final dxfs ready for export to EBL and prepare them for writes on one of the two Elionix instruments at Harvard.
     Export modes can be without global alignment (simple unaligned write, good for the first EBL layer), with global alignment, or with local and global alignment.
            Note that only 4 point alignment is available at this time. If you are going to do alignment, might as well.
     All layers in "active layers" list will be written.

While this code was built to deal with matrixed device writing for semiconductor fab with local and global alignment, it can also be used for simple 1-off CAD files.
Simply set num_cols=num_rows=1 and make sure your device is surrounded by a bounding box called matrix.
This code can be easily modified to toggle other parameters in this simple flow, or to accomadate other "standard" flows for whatever the user deems to be standard.
See the BEAMER docs for how to export a flow to python gobj in order to integrate into this notebook (almost as easy as drag and drop, just need to worry about variable renaming).
To implment not on the Harvard BEAMER computer, you will need to change the paths for the BEAMER exe, lib, and psf lib. 
See the companion DXFPreprocessing Script to generate matrixed device chips.
This code plots the nearest version of the input DXF used as an input to the BEAMER flow. However, checking the EBL file output file is read by the EBL software is always reccomended.

PLEASE REPORT BUGS to aksaydjari@gmail.com (suggestions/code contributions are also appreciated)

## Installation

Simply use conda to create a new enviornment using the yml file provided. See the conda documentation. 
Use of miniconda instead of full Anaconda is suggested.

## Version Control
Please download the most recent version from
https://github.com/andrew-saydjari/DXFProcessors
