# Print Manager

Print Manager is a utility program installed by [Print Studio](/developers/reference/desktop-applications/print-studio) that manages its 3D printer connectivity. 

The Print Manager has a console you can load from your app and runs a local version of the Spark API that can be called without requiring an internet connection.

If you have Spark-compatible 3D printers connected by USB or over a network with Bonjour (on Macs), Print Manager will detect their presence and Print Studio will provide you with options to send your file to a printer. 

If you are a 3D printer manufacturer<span> and want to integrate a 3D printer with Spark, please refer to [this guide](/developers/reference/printer-manufacturers/integrate-your-printer/integrate-your-printer-model "Printer Manufacturers").</span>

* * *

**Benefits of Print Manager Integration:**

1.  Can be opened directly from your app.
2.  Automatically detects and connects with local 3D printers.
3.  Creates and manages 3D print-jobs.
4.  Monitors print job execution.
5.  Can be used to call Print APIs locally.
6.  Keeps a log of its activity. The location of the log can be set in the advanced options.

### Loading the Print Manager Console

**To open** the Print Manager Console redirect to_** [http://localhost:9998/console](http://localhost:9998/console)**_.  This opens the Print Manager's _Status_ screen.

*   **Manage your printers** -  Click the _Edit_ button (top right), this opens the Manage Printers screen.
    *   Adds a new printer.
    *   Sets the IP address of the printer.
*   **Set advanced options** - Click the _Settings_ button (top left), this opens the Advanced Options screen.
    *   Sets the port that the console runs from (the default is 9998).
    *   Sets the log file name and location.

###  ![x1](https://s3.amazonaws.com/spark-dev-portal-prd/blog-alpha-content/uploads/2015/05/x16.png)

### Using Print Manager to call Print APIs

Print Manager service/daemon implements our API server on your desktop. It can be used to call Spark Print APIs directly, without loading Print Studio or connecting to the cloud; simply replace the _URL prefix_ with **http://localhost:9998**.

**`GET _https://sandbox.spark.autodesk.com/api/v1/print/printers_`**  
is replaced with  
**`_GET http://localhost:9998/print/printers_`**

Print Manager runs a version of the Drive API's **file upload** and **file download** on your local machine, but does not provide access to cloud data or cloud APIs such as the _Drive API_ or _Service Bureau API_. Assets stored on the Spark servers will require a regular API call.

Printer Registration and Print Job Creation for local printers use different parameters from cloud printers.

See the [Print API](https://spark.autodesk.com/developers/reference/print) documentation for a list of commands. 
