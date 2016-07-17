# Print Studio


Autodesk Print Studio is a desktop application that offers sophisticated 3D print preparation and printer delivery services for OBJ and STL models. 

To install Print Studio see the [Print Studio download page](https://github.com/spark3dp/print-manager/releases).

If you are a 3D printer manufacturer<span> and want to integrate a 3D printer with Spark, please refer to [this guide](/developers/reference/printer-manufacturers/integrate-your-printer/integrate-your-printer-model "Printer Manufacturers").</span>

Print Studio can be run as stand-alone software or integrated with your app to open a pre-loaded OBJ file, STL file or a Spark Tray object.  

Print Studio installs a Windows_service / _Mac _daemon_ called the [Print Manager](/developers/reference/desktop-applications/print-manager)**.** Print Manager can be integrated into your app as a stand alone 3D printer manager able to automatically detect and connect to local 3D printers. Print Manager can also be used to run Spark APIs locally (without connecting to the internet).

* * *

**Benefits of Print Studio Integration** 

1.  Opens from an app with a pre-loaded STL or OBJ file. 
2.  Automatically locates and heals 3D geometry errors.
3.  Optimizes models for 3D printing using preset print-profiles or manually generated user-defined print-profiles:
    *   Rotates, sizes and places the model in the build platform.
    *   Calculates support material requirements.
    *   Optimizes slicing settings.
4.  Calculates printing time and material requirements.
5.  Automatically detects and connects local Spark-enabled 3D printers (using the Print Manager service, see below).
6.  Can send the printer-ready file to a connected Spark-enabled 3D printer or download it for further work.
7.  Monitors and controls print-job execution on connected 3D printers.

![Autodesk Print Studio](https://dp6mb85fgupxl.cloudfront.net/blog-prd-content/uploads/2015/07/05_preview_v02.png)

## Opening Print Studio from your app

To be called from an app, Print Studio must be installed by the app-user.

**Print Studio command-line options:**

1.  **-import _[file path and name]_** - Import the specified OBJ or STL file on startup.  By default, the units and up-vector will be read for Print Studio's preference settings. The [file path and name] parameter is handled as a second parameter and must contain the path to the file and have a .STL or .OBJ suffix. 
    *   Example of a Mac script opening a file (uses \\ to indicate spaces in the name): 

        <pre class="p1"><span class="s1">**do shell script**</span> <span class="s2">"open /Applications/Autodesk/Autodesk\\ Print\\ Studio/Print\\ Studio.app --args -import /Applications/Autodesk/mymodel.stl -units cm"</span></pre>

    *   <span class="s2">Example of a Mac terminal command line opening a file (uses \ to indicate spaces in the name):</span>

        <pre class="p1"><span class="s2">open /Applications/Autodesk/Autodesk\ Print\ Studio/Print\ Studio.app --args -import</span> /Applications/Autodesk/mymodel.stl -units mm </pre>

2.  **-yup** - Override Print Studio preference settings and read the imported model as if the y-axis is the up-vector.
3.  **-zup** - Override Print Studio preference settings and read the imported model as if the z-axis is the up-vector.
4.  **-units _[units]_** - Override Print Studio preference settings and read the imported model as if it is expressed in the specified units.  The [units] is handled as a second parameter, valid values for are 'mm', 'cm' or 'in'.
5.  **-tray _[tray ID]_ - **Load the specified tray ID from the Spark server.  Note that the -yup, -zup, and -units options cannot be used with the -tray option. The [tray ID] is handled as a second parameter and must contain the Spark ID of a tray object.
