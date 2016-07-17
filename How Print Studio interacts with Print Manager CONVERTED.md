# How Print Studio interacts with Print Manager

In this guide, we explain in depth how Print Studio and Print Manager work together. If you are interested in integrating your printer with Spark Print Manager and Print Studio, you may want to read the topic [Integrate Your Printer Model](https://spark.autodesk.com/developers/reference/printer-manufacturers/integrate-your-printer/integrate-your-printer-model).
Autodesk Print Studio makes multiple calls to Print Manager to get the list of printer types, materials, profiles, and connected printers. However, the first call Print Studio makes at start up is this call:

<a href="http://localhost:9998/version">http://localhost:9998/version</a>

This call returns the following data:

``{  
   "api":{  
      "major":1,
      "minor":0,
      "commit_id":"0000000000000000000000000000000000000000",
      "short_commit_id":"000000"
   },
   "core_technology":{  
      "commit_id":"4ca0192f5cfcf0960cbf33be0004b467ff89d219",
      "format_version":6,
      "short_commit_id":"4ca019"
   },
   "print_definition":{  
      **"schema":4,
      "revision":7**
   }
}``

The schema and revision numbers indicate whether Print Studio's cached database needs updating. If the returned schema / revision number combination is higher than the one stored in Print Studio's cached database, Print Studio requests new printer types, materials and profiles from Print Manager and stores this data and the version information in a new cached database.
If the version information is the same as the version information in its cached database, Print Studio uses the information already retrieved from the cached database.
Therefore, if you are making changes to print manager printer types, profiles and materials you need to change this version information in the version file [https://github.com/spark3dp/print-manager/blob/master/spark-print-data/version.json](https://github.com/spark3dp/print-manager/blob/master/spark-print-data/version.json).

TIP: It's common to make many changes to the database during development. To avoid the tedious task of bumping the version information every time you need to preview outcomes, you can delete the cached database, forcing Print Studio to retrieve and use the new data. The location of the cached database is:

For Windows: \AppData\Local\Autodesk\Print Studio\printDB
For MacOS /Users//Library/Application Support/com.autodesk.spark.printstudio/printDB

If the version information indicates that the local cache needs to be updated, Print Studio makes these three calls:

<a href="http://localhost:9998/printdb/printertypes">http://localhost:9998/printdb/printertypes</a>

<a href="http://localhost:9998/printdb/profiles">http://localhost:9998/printdb/profiles</a>

<a href ="http://localhost:9998/printdb/materials">http://localhost:9998/printdb/materials</a>

The data returned from these calls is stored in the JSON files listed below, which are part of the Print Manager data.
Edit these files in order to add a printer.

[https://github.com/spark3dp/print-manager/blob/master/spark-print-data/materials.json](https://github.com/spark3dp/print-manager/blob/master/spark-print-data/materials.json)
[https://github.com/spark3dp/print-manager/blob/master/spark-print-data/printertypes.json](https://github.com/spark3dp/print-manager/blob/master/spark-print-data/printertypes.json)
[https://github.com/spark3dp/print-manager/blob/master/spark-print-data/profiles.json](https://github.com/spark3dp/print-manager/blob/master/spark-print-data/profiles.json)

Once these files have been edited, Print Studio can display the new printer's profile.

[[{"fid":"281","view_mode":"wysiwyg","type":"media","attributes":{"height":"900","width":"1440","class":"media-element file-wysiwyg"}}]]

Print Studio can also display the printers that are connected. To do this, it calls Print Manager:

<a href="http://localhost:9998/print/printers">http://localhost:9998/print/printers</a>

This call returns all the printers connected to Print Manager by USB, network, and serial connections,
enabling Print Studio to display the connected printers in the Printers dialog:

[[{"fid":"286","view_mode":"wysiwyg","type":"media","attributes":{"height":"802","width":"1044","class":"media-element file-wysiwyg"}}]]

After the printer has been selected, Print Studio is ready to print. When Print Studio's Print or Export buttons are clicked,
Print Manager is activated.

Print Studio then generates the correct neutral printable format for the supported DLP or FDM printer being used,
according to that printer's definition in the printertypes.json file where each “printer type” is declared to be either

"technology": "DLP"

or

"technology": "FDM"

Setting the printer technology allows Print Manager to generate the appropriate neutral file format for the selected printer.
Once this format is generated, Print Studio makes this call to Print Manager:

<a href="http://localhost:9998/print/trays/translate">http://localhost:9998/print/trays/translate</a>

with these inputs:

Job Name

File ID (the neutral file)

Printer Type ID

Profile ID

Material ID

As a response to this call, Print Manager uses the Printer Type and ID to generate the printer file appropriate for the target printer.
The name of this file is the returned value of the command, and the file is sent to the printer by Print Studio.

See the readme [https://github.com/spark3dp/print-manager/blob/master/spark-print-mgr/printableTranslation/README.md](https://github.com/spark3dp/print-manager/blob/master/spark-print-mgr/printableTranslation/README.md).
for more information about Spark Printable File Format Conversion and Translation for 3D Printers.

Once the printable file has been generated and sent to the printer, Print Studio commands Print Manager to create the job using the call:

<a href="http://localhost:9998/print/jobs">http://localhost:9998/print/jobs</a>

The input for this call includes the Printer ID, i.e. the ID of one of the connected printers.
The response returns a job ID.

When that job is created, the printable (file) is set using this call:

<a href="http://localhost:9998/print/jobs/100/setPrintable">http://localhost:9998/print/jobs/100/setPrintable</a>

with this input:

file_id (the printable file)

Finally the printer is started using this call:

<a href="http://localhost:9998/print/printers/10/print">http://localhost:9998/print/printers/10/print</a>

Control is handed over to Print Manager, which controls the job from this point on.
Once the job has been handed over to the Print Manager, its status is displayed at:

<a href="http://localhost:9998/console/#/printers">http://localhost:9998/console/#/printers</a>.
