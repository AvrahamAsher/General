<h1>Integrate Your Printer Model</h1>

<p class="highlight_box ng-scope">This document is for <strong>3D printer manufacturers</strong>&nbsp;who wish to integrate a printer model&nbsp;with Print Manager desktop utility. You should read the <a href="/developers/reference/desktop-applications/print-manager" target="_blank">Print Manager</a>&nbsp;documentation before reading this page.</p>

<p class="ng-scope"><strong>Print Manager</strong>&nbsp;is an open-source<span>&nbsp;utility program used by </span><a href="/developers/reference/desktop-applications/print-studio" target="_blank">Print Studio</a>, Print Manager&nbsp;supports FDM and DLP printers<span>.&nbsp;</span></p>

<p class="ng-scope">The Print Manager workflow:</p>

<ol class="ng-scope">
	<li><strong>List 3D printers</strong> - Print Studio users see a list of 3D printers that they can use. &nbsp;The list is generated by Print Manager, and can be viewed in the Print Manger Console without running Print Studio.</li>
	<li><strong>Add new printer</strong> - Print Manager may receive a request to connect&nbsp;a new 3D printer&nbsp;to Print Studio or to&nbsp;the Console. The new printer will be connected by USB, serial connection or IP address.</li>
	<li><span><strong>Receive instructions for physical model</strong>&nbsp;- When a Print Studio user submits a 3D print,&nbsp;Print Manager is sent a&nbsp;file of instructions&nbsp;for generating the&nbsp;physical model. The instructions are&nbsp;not specific to any 3D printer model and arrive in&nbsp;different formats for DLP and FDP printers.&nbsp;</span></li>
	<li><span><strong>Translate the instructions into a printable file</strong>&nbsp;- Print Manager&nbsp;<em>translates</em> the received&nbsp;instructions&nbsp;into a&nbsp;<em>printable file</em>&nbsp;specific to the selected 3D printer model.</span></li>
	<li><strong>Send the printable&nbsp;file to the printer</strong> - Print Manager sends the printable&nbsp;file to the selected 3D printer&nbsp;through the following&nbsp;transports: IP (address or Mac Bonjour discovery), serial port or USB port.</li>
</ol>

<p class="ng-scope">For more information see <a href="/developers/reference/desktop-applications/print-manager" target="_blank">Print Manager</a>.&nbsp;Print Manager's source code is available at&nbsp;<a href="https://github.com/spark3dp/print-manager" target="_blank">https://github.com/spark3dp/print-manager</a>.</p>

<p class="ng-scope">Print Manager creates a local version of the Spark API that can be run on any desktop computer and does not require an internet connection.&nbsp;</p>

<hr class="ng-scope" />
<h2 class="ng-scope">How to integrate&nbsp;your 3D printer with&nbsp;the Print Manager utility</h2>

<ol>
	<li><a class="ng-isolate-scope" href="#listing">List your printer in Print Manager</a>, so that Print Studio users will see it.</li>
	<li><a class="ng-isolate-scope" href="#communicating">Communicate with Print Manager</a>, so your 3D printer can receive and send data.</li>
	<li><a class="ng-isolate-scope" href="#add">Add a translator for your 3D printer</a>, so the file of&nbsp;printer commands&nbsp;output by Print Studio are converted into instructions recognized by your printer.</li>
	<li><a class="ng-isolate-scope" href="#addingprinter">Add your&nbsp;printer to the console</a>, so that it can be monitored and controlled (start, pause, cancel) from it.</li>
	<li>
	<p><a class="ng-isolate-scope" href="#testing">Test your integration</a>. &nbsp;</p>
	</li>
</ol>

<p class="ng-scope">Once your Print Manager integration is working you can submit a pull request in GitHub to have us integrate it into the general distribution of Print Manager.</p>

<p class="ng-scope">&nbsp;</p>

<h2 class="western ng-scope" id="listing">1. Listing&nbsp;Your&nbsp;Printer in Print Manager</h2>

<p class="ng-scope"><span>Print Manager is installed with&nbsp;Print Studio; We advise&nbsp;you not to use installed versions and to download the code from&nbsp;</span><b><span><a href="https://github.com/spark3dp/print-manager" target="_blank">https://github.com/spark3dp/print-manager</a>&nbsp;</span></b>for use in testing or creating your own version. These instructions assume you have download the source onto your machine.</p>

<ol class="ng-scope">
	<li class="western"><span color="#00000a"><span color="#00000a"><span>If it is installed, you may need to&nbsp;stop the Print Manager service (Windows) or daemon (Mac) before adding your printer. Stopping&nbsp;this service in your operating system ensures&nbsp;that all tests&nbsp;will use your modified version of Print Manager.&nbsp;</span></span></span><span color="#00000a"><span>Print Manager can be configured to run on a different port&nbsp;from within&nbsp;the advanced settings.</span></span></li>
	<li class="western">Create&nbsp;the following files for every printer you add:
	<ol style="list-style-type: lower-alpha;">
		<li><strong>A print bed model</strong>, placed in the&nbsp;<a href="https://github.com/spark3dp/print-manager/tree/master/spark-print-data/data" target="_blank">data&nbsp;folder</a>&nbsp;of&nbsp;the Print Manager source location<em>. </em>For more information, <a class="ng-isolate-scope" href="#appendix">see the appendix</a>.</li>
		<li><strong>Icon and printer brand images</strong>,&nbsp;<span>placed in the&nbsp;<a href="https://github.com/spark3dp/print-manager/tree/master/spark-print-data/data" target="_blank">data folder</a>&nbsp;of&nbsp;the&nbsp;Print Manager&nbsp;source location</span><em>.&nbsp;</em>For more information, <a class="ng-isolate-scope" href="#appendix">see the appendix</a>.</li>
		<li><strong>Printer properties</strong>&nbsp;added to the <em>PrinterTypes.json</em> file in the&nbsp;<a href="https://github.com/spark3dp/print-manager/tree/master/spark-print-data" target="_blank">spark-print-data&nbsp;folder</a><span>&nbsp;of the&nbsp;</span>Print Manager&nbsp;source location.&nbsp;For more information, <a class="ng-isolate-scope" href="#appendix">see the appendix</a>.</li>
		<li><strong>Materials used</strong> must exist in the <em>materials.json</em> file in the same location as the PrinterTypes.json file.&nbsp;<span>For more information about this file, see the </span><a href="/developers/reference/print?deeplink=%2Freference%2Fprint-definitions%2Fmaterials" target="_blank">Print API documentation</a><span>.</span></li>
		<li><strong>Default settings</strong> must exist in the <em>profile.json</em> file in the same location as the PrinterTypes.json file. For more information about this file, see the <a href="/developers/reference/print?deeplink=%2Freference%2Fprint-definitions%2Fprofiles" target="_blank">Print API documentation</a>.</li>
	</ol>
	</li>
	<li class="western">To start the server type $ node server.js (for more information see the <a href="https://github.com/spark3dp/print-manager" target="_blank">readme&nbsp;on GitHub</a>).</li>
	<li class="western"><span color="#00000a"><span>The server will be started on localhost:9998, where Print Studio looks for&nbsp;it. Use Print Manager's&nbsp;advanced settings to set the port on&nbsp;which Print Studio locates Print Manager.</span></span></li>
</ol>

<h2 class="ng-scope" id="communicating">2. Communicating with Print Manager</h2>

<p class="ng-scope">Print Manager automatically discovers&nbsp;serial and USB printers.&nbsp;</p>

<ol class="ng-scope" style="list-style-type: lower-alpha;">
	<li><strong>Activate Printer Discovery</strong> - Printer discovery is handled by <em>usbDiscovery.js</em> and <em>serialDiscovery.js</em> in the <a href="https://github.com/spark3dp/print-manager/tree/master/spark-print-mgr/printers" target="_blank">Printers folder</a>. &nbsp;These scripts fire a <strong>deviceUp event</strong> which is monitored in <em>PrintManager.js</em>. We expect to receive the following fields: Servicename, Identifer and deviceType.<br />
	<span><span>If your printer is not USB or serial, y</span>ou must&nbsp;add a reference to your driver in the<em> <a href="https://github.com/spark3dp/print-manager/blob/master/spark-print-mgr/printers/printerUtil.js" target="_blank">PrintUtils.js</a></em>&nbsp;file. USB and serial printers need to be referenced in the <a href="https://github.com/spark3dp/print-manager/blob/master/spark-print-mgr/printers/drivers/driverConfig.json" target="_blank">Driverconfig.json file</a>.</span></li>
	<li><strong>Map Printer Commands</strong> - The commands sent and received in Print Manager can be found in&nbsp;<a href="https://github.com/spark3dp/print-manager/blob/master/spark-print-mgr/printers/command.js" target="_blank">command.js</a>&nbsp;and need to be mapped in your driver which must be placed in the <a href="https://github.com/spark3dp/print-manager/tree/master/spark-print-mgr/printers/drivers" target="_blank">driver folder</a>.</li>
</ol>

<h2 class="ng-scope" id="add">3. Translating&nbsp;Print Studio&nbsp;Output for Your Printer</h2>

<p class="ng-scope">Print Studio&nbsp;outputs a file to the Print Manager containing&nbsp;<span>instructions&nbsp;for generating the&nbsp;physical model</span>. The file contains a series of commands for the 3D printer in an FDM or DLP format that is not specific to any make of printer.</p>

<ul class="ng-scope">
	<li>To view the commands output by Print Studio, see the&nbsp;files&nbsp;<a href="https://github.com/spark3dp/print-manager/blob/master/spark-print-mgr/printableTranslation/FDMPrintable.proto" target="_blank">FDMPrintable.proto</a>&nbsp;(FDM commands)&nbsp;and&nbsp;<a href="https://github.com/spark3dp/print-manager/blob/master/spark-print-mgr/printableTranslation/DLPPrintable.proto" target="_blank">DLPPrintable.proto</a>&nbsp;(DLP commands).</li>
</ul>

<p class="ng-scope">Print Manager&nbsp;translates the Print Studio file into a printable file suitable for your printer. If your printer is not supported, you must add your translator.</p>

<ul class="ng-scope">
	<li>Spark GCODE translation is documented at&nbsp;<a href="https://github.com/spark3dp/print-manager/blob/master/spark-print-mgr/printableTranslation" target="_blank">https://github.com/<wbr />spark3dp/print-manager/blob/<wbr />master/spark-print-mgr/<wbr />printableTranslation</a>.

	<ul>
		<li>FDM translation<strong> -&nbsp;</strong>The FDM translator&nbsp;code can be found&nbsp;at <a href="https://github.com/spark3dp/print-manager/blob/master/spark-print-mgr/printableTranslation/FDMTranslator.js" target="_blank">https://github.com/spark3dp/<wbr />print-manager/blob/master/<wbr />spark-print-mgr/<wbr />printableTranslation/<wbr />FDMTranslator.js</a>.<br />
		Translators exist&nbsp;for Makerbot, Dremel, Ultimaker and Typea. The source of these translators can be viewed at&nbsp;<a href="https://github.com/spark3dp/print-manager/blob/master/spark-print-mgr/printableTranslation/translators" target="_blank">https://github.com/<wbr />spark3dp/print-manager/blob/<wbr />master/spark-print-mgr/<wbr />printableTranslation/<wbr />translators</a>/.</li>
		<li>DLP translation<strong> -&nbsp;</strong>A&nbsp;translator converts this to the needs of the&nbsp;Ember Printer:&nbsp;see the source at&nbsp;<a href="https://github.com/spark3dp/print-manager/blob/master/spark-print-mgr/printableTranslation/translators/Autodesk-Ember.js" target="_blank">https://github.com/<wbr />spark3dp/print-manager/blob/<wbr />master/spark-print-mgr/<wbr />printableTranslation/<wbr />translators/Autodesk-Ember.js</a>.&nbsp;<br />
		Ember requires&nbsp;a <em>tar.gz</em> file combining the&nbsp;slicing&nbsp;and printer settings.</li>
	</ul>
	</li>
</ul>

<h2 class="ng-scope" id="addingprinter">4.&nbsp;Adding Your&nbsp;Printer to the Console</h2>

<p>The console application is a simple web application that displays enabled printers and offers simple printer monitoring and control (start, pause, cancel).</p>

<p>To add&nbsp;a printer to the console:</p>

<p>1. Edit&nbsp;https://github.com/spark3dp/print-manager/blob/sandbox/spark-print-mgr/console/js/controllers.js</p>

<p>2. Add your printer (UUID, cssClass and nickname) to the list of printers as in the following example:</p>

<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; '8301C8D0-7A59-4F4B-A918-D5D38888790F' : {</p>

<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; cssClass: 'printrbotplus',</p>

<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; nickname: 'Printrbot Plus'</p>

<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; }</p>

<p>&nbsp; &nbsp; &nbsp; &nbsp;Note:</p>

<p>&nbsp; &nbsp; &nbsp; &nbsp;UUID is the printer type ID.</p>

<p>&nbsp; &nbsp; &nbsp; &nbsp;cssClass is a reference to https://github.com/spark3dp/print-manager/blob/sandbox/spark-print-mgr/console/css/console.css</p>

<p>3. Add an image file in PNG format to represent your printer in the console display.</p>

<p>Example:</p>

<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; '.printrbotplus {</p>

<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; background-image: url("../icons/printrbotplus.png");</p>

<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; }</p>

<p>4. Save https://github.com/spark3dp/print-manager/blob/sandbox/spark-print-mgr/console/js/controllers.js.</p>

<p>5. Restart your console and verify that your printer is displayed.</p>

<h2 class="ng-scope" id="testing">5.&nbsp;Testing&nbsp;your integration</h2>

<p class="ng-scope">The following text matrix is a general guideline. additional tests may be required.</p>

<table class="table1 ng-scope" style="padding-left: 30px;">
	<tbody>
		<tr>
			<td style="margin: 3px; padding: 3px; background-color: #86bee5; border-bottom-style: none;">&nbsp;</td>
			<td colspan="3" style="margin: 3px; padding: 3px; background-color: #86bee5;">Printer Model A</td>
			<td colspan="3" style="margin: 3px; padding: 3px; background-color: #86bee5;">Printer Model B</td>
		</tr>
		<tr>
			<td style="margin: 5px; padding-top: 5px; padding-right: 5px; padding-bottom: 5px; border-top-style: none; background-color: #86bee5;"><strong>Test Description</strong></td>
			<td style="margin: 7px; padding: 7px; background-color: #86bee5; white-space: nowrap;">Win 7</td>
			<td style="margin: 7px; padding: 7px; background-color: #86bee5; white-space: nowrap;">Win 8</td>
			<td style="margin: 7px; padding: 7px; background-color: #86bee5; white-space: nowrap;">Mac</td>
			<td style="margin: 7px; padding: 7px; background-color: #86bee5; white-space: nowrap;">Win 7</td>
			<td style="margin: 7px; padding: 7px; background-color: #86bee5; white-space: nowrap;">Win 8</td>
			<td style="margin: 7px; padding: 7px; background-color: #86bee5; white-space: nowrap;">Mac</td>
		</tr>
		<tr>
			<td style="font-size: 12px; margin: 3px; padding: 3px;">Connect printer to computer: Verify that Print Studio recognizes its presence.</td>
			<td>&nbsp;</td>
			<td>&nbsp;</td>
			<td>&nbsp;</td>
			<td>&nbsp;</td>
			<td>&nbsp;</td>
			<td>&nbsp;</td>
		</tr>
		<tr>
			<td style="font-size: 12px; margin: 3px; padding: 3px;">Verify graphics in Print Studio.</td>
			<td>&nbsp;</td>
			<td>&nbsp;</td>
			<td>&nbsp;</td>
			<td>&nbsp;</td>
			<td>&nbsp;</td>
			<td>&nbsp;</td>
		</tr>
		<tr>
			<td style="font-size: 12px; margin: 3px; padding: 3px;">Send a job (a 3D print task) to the printer and complete job 100%: Verify UI is accurate.</td>
			<td>&nbsp;</td>
			<td>&nbsp;</td>
			<td>&nbsp;</td>
			<td>&nbsp;</td>
			<td>&nbsp;</td>
			<td>&nbsp;</td>
		</tr>
		<tr>
			<td style="font-size: 12px; margin: 3px; padding: 3px;">Resend the same job to the printer after the first one completes.</td>
			<td>&nbsp;</td>
			<td>&nbsp;</td>
			<td>&nbsp;</td>
			<td>&nbsp;</td>
			<td>&nbsp;</td>
			<td>&nbsp;</td>
		</tr>
		<tr>
			<td style="padding-left: 30px; font-size: 12px; margin: 3px; padding: 3px;">Pause the job at 50%.</td>
			<td>&nbsp;</td>
			<td>&nbsp;</td>
			<td>&nbsp;</td>
			<td>&nbsp;</td>
			<td>&nbsp;</td>
			<td>&nbsp;</td>
		</tr>
		<tr>
			<td style="padding-left: 30px; font-size: 12px; margin: 3px; padding: 3px;">Resume the print.</td>
			<td>&nbsp;</td>
			<td>&nbsp;</td>
			<td>&nbsp;</td>
			<td>&nbsp;</td>
			<td>&nbsp;</td>
			<td>&nbsp;</td>
		</tr>
		<tr>
			<td style="padding-left: 30px; font-size: 12px; margin: 3px; padding: 3px;">Stop/cancel the print at 75%.</td>
			<td>&nbsp;</td>
			<td>&nbsp;</td>
			<td>&nbsp;</td>
			<td>&nbsp;</td>
			<td>&nbsp;</td>
			<td>&nbsp;</td>
		</tr>
		<tr>
			<td style="font-size: 12px; margin: 3px; padding: 3px;">Resend the print again and complete job 100%.</td>
			<td>&nbsp;</td>
			<td>&nbsp;</td>
			<td>&nbsp;</td>
			<td>&nbsp;</td>
			<td>&nbsp;</td>
			<td>&nbsp;</td>
		</tr>
		<tr>
			<td style="font-size: 12px; margin: 3px; padding: 3px;">Restart the computer with the 3D printer still connected and open Print Studio: Verify that the printer is still visible.</td>
			<td>&nbsp;</td>
			<td>&nbsp;</td>
			<td>&nbsp;</td>
			<td>&nbsp;</td>
			<td>&nbsp;</td>
			<td>&nbsp;</td>
		</tr>
		<tr>
			<td style="font-size: 12px; margin: 3px; padding: 3px;">Send a job to the 3D printer and complete the job 100%.</td>
			<td>&nbsp;</td>
			<td>&nbsp;</td>
			<td>&nbsp;</td>
			<td>&nbsp;</td>
			<td>&nbsp;</td>
			<td>&nbsp;</td>
		</tr>
		<tr>
			<td style="font-size: 12px; margin: 3px; padding: 3px;">Connect multiple 3D printers of same kind: Verify that they show up in the UI.</td>
			<td>&nbsp;</td>
			<td>&nbsp;</td>
			<td>&nbsp;</td>
			<td>&nbsp;</td>
			<td>&nbsp;</td>
			<td>&nbsp;</td>
		</tr>
		<tr>
			<td style="padding-left: 30px; font-size: 12px; margin: 3px; padding: 3px;">Send a print to one of the connected printers and complete the print Job.</td>
			<td>&nbsp;</td>
			<td>&nbsp;</td>
			<td>&nbsp;</td>
			<td>&nbsp;</td>
			<td>&nbsp;</td>
			<td>&nbsp;</td>
		</tr>
		<tr>
			<td style="padding-left: 30px; font-size: 12px; margin: 3px; padding: 3px;">Restart the computer: Verify that all printers remain connected.</td>
			<td>&nbsp;</td>
			<td>&nbsp;</td>
			<td>&nbsp;</td>
			<td>&nbsp;</td>
			<td>&nbsp;</td>
			<td>&nbsp;</td>
		</tr>
		<tr>
			<td style="padding-left: 30px; font-size: 12px; margin: 3px; padding: 3px;">Disconnect all the printers: Verify that they are all gone.</td>
			<td>&nbsp;</td>
			<td>&nbsp;</td>
			<td>&nbsp;</td>
			<td>&nbsp;</td>
			<td>&nbsp;</td>
			<td>&nbsp;</td>
		</tr>
	</tbody>
</table>

<h2 class="ng-scope" id="appendix">Appendix:&nbsp;Files Used in Print Manager</h2>

<h3 class="ng-scope">1.&nbsp;Print bed model</h3>

<p class="ng-scope" style="padding-left: 30px;">The print bed model is composed of a number of files contained in a zip file and&nbsp;<span>placed in the&nbsp;<a href="https://github.com/spark3dp/print-manager/tree/master/spark-print-data/data" target="_blank">data folder</a></span><span>&nbsp;of Print Manager</span>:</p>

<ol class="ng-scope" style="list-style-type: lower-alpha;">
	<li><strong>A geometry file</strong> in obj format modelling the print bed dimensions.</li>
	<li><span style="color: #000000;"><strong>An associated materials&nbsp;file</strong>&nbsp;(in mtl format) providing additional rendering information.</span></li>
	<li><span style="color: #000000;"><strong>Texture files</strong> as required, may include printer logos.</span></li>
</ol>

<p class="ng-scope" style="padding-left: 30px;">Both the zip file and&nbsp;its content files must have the same name. &nbsp;For example:</p>

<ul class="ng-scope">
	<li style="padding-left: 30px;">PrinterName.zip&nbsp;
	<ul>
		<li style="padding-left: 30px;">PrinterName.obj</li>
		<li style="padding-left: 30px;">PrinterName.mtl</li>
		<li style="padding-left: 30px;">PrinterName_texture.png</li>
	</ul>
	</li>
</ul>

<p class="ng-scope" style="padding-left: 30px;">The&nbsp;polygonal model of the print bed has&nbsp;the following features:</p>

<ul class="ng-scope">
	<li>As few&nbsp;triangles as possible</li>
	<li>Normals pointing to the outside.</li>
	<li>Normals being saved as face-normals (to avoid smooth shading of the print bed geometry).</li>
	<li>Correct bed dimensions (not the build volume dimensions).</li>
	<li>Save the file in *.obj format with .mtl textures assigned.</li>
	<li><span style="color: #000000;">We recommend that any branding should be saved in the&nbsp;texture files and not in the model. See the Dremel, Ember and Ultimaker <a href="https://github.com/spark3dp/print-manager/tree/master/spark-print-data/data" target="_blank">examples in GitHub</a>.&nbsp;</span></li>
</ul>

<p class="ng-scope">&nbsp;</p>

<h3 class="ng-scope">2.&nbsp;Icons and printer brand images</h3>

<p class="ng-scope" style="padding-left: 30px;">Spark requires the following visuals for use in the Print Manager interface, for each type of&nbsp;printer, <span>placed in the&nbsp;<a href="https://github.com/spark3dp/print-manager/tree/master/spark-print-data/data" target="_blank">data folder</a><em>&nbsp;</em>of Print Manager:</span></p>

<table class="ng-scope">
	<tbody>
		<tr>
			<td style="margin: 3px; padding: 3px; background-color: #86bee5;"><strong>File Description</strong></td>
			<td style="margin: 3px; padding: 3px; background-color: #86bee5;"><strong>Format</strong></td>
			<td style="margin: 3px; padding: 3px; background-color: #86bee5;"><strong>Dimensions</strong></td>
			<td style="margin: 3px; padding: 3px; background-color: #86bee5;"><strong>File Name</strong></td>
		</tr>
		<tr>
			<td style="font-size: 12px; margin: 3px; padding: 3px;">Printer branding</td>
			<td style="font-size: 12px; margin: 3px; padding: 3px;">Png</td>
			<td style="font-size: 12px; margin: 3px; padding: 3px;">454x90</td>
			<td style="font-size: 12px; margin: 3px; padding: 3px;"><span><printer-name>my-printers-name.png</printer-name></span></td>
		</tr>
		<tr>
			<td style="font-size: 12px; margin: 3px; padding: 3px;">Default icon to be sent to the printer</td>
			<td style="font-size: 12px; margin: 3px; padding: 3px;">Defined by printer firmware</td>
			<td style="font-size: 12px; margin: 3px; padding: 3px;">Defined by printer firmware</td>
			<td style="font-size: 12px; margin: 3px; padding: 3px;">&nbsp;</td>
		</tr>
		<tr>
			<td style="font-size: 12px; margin: 3px; padding: 3px;">Printer icon&nbsp;(non-retina)</td>
			<td style="font-size: 12px; margin: 3px; padding: 3px;">Png</td>
			<td style="font-size: 12px; margin: 3px; padding: 3px;">50x50</td>
			<td style="font-size: 12px; margin: 3px; padding: 3px;"><span><printer-name>my-printers-name50x50.png</printer-name></span></td>
		</tr>
		<tr>
			<td style="font-size: 12px; margin: 3px; padding: 3px;">Printer icon&nbsp;(retina)</td>
			<td style="font-size: 12px; margin: 3px; padding: 3px;">Png</td>
			<td style="font-size: 12px; margin: 3px; padding: 3px;">100x100</td>
			<td style="font-size: 12px; margin: 3px; padding: 3px;"><span><printer_name>my-printers-name100x100.png</printer_name></span></td>
		</tr>
	</tbody>
</table>

<h3 class="ng-scope">&nbsp;</h3>

<h3 class="ng-scope">3.&nbsp;Printer properties</h3>

<p class="ng-scope" style="padding-left: 30px;">Printer properties are defined in the <em>printerTypes.JSON</em> file, which is located in the <a href="https://github.com/spark3dp/print-manager/tree/master/spark-print-data" target="_blank">Spark-print-data&nbsp;folder</a>&nbsp;of Print Manager.<em> </em>These are the fields:</p>

<table class="table1 ng-scope" style="padding-left: 30px;">
	<tbody>
		<tr>
			<td style="margin: 3px; padding: 3px; background-color: #86bee5;"><strong>Field name</strong></td>
			<td style="margin: 3px; padding: 3px; background-color: #86bee5;"><strong>Type</strong></td>
			<td style="margin: 3px; padding: 3px; background-color: #86bee5;"><strong>Description</strong></td>
			<td style="margin: 3px; padding: 3px; background-color: #86bee5;"><strong>Example value</strong></td>
		</tr>
		<tr>
			<td style="font-size: 12px; margin: 3px; padding: 3px;">id</td>
			<td style="font-size: 12px; margin: 3px; padding: 3px;"><a href="https://en.wikipedia.org/wiki/Universally_unique_identifier" target="_blank">UUID</a></td>
			<td style="font-size: 12px; margin: 3px; padding: 3px;">A random number.</td>
			<td style="font-size: 12px; margin: 3px; padding: 3px;">"3F64F6EC-A1DF-44AB-A22E-58C036F2F475"</td>
		</tr>
		<tr>
			<td style="font-size: 12px; margin: 3px; padding: 3px;">version</td>
			<td style="font-size: 12px; margin: 3px; padding: 3px;">numeric</td>
			<td style="font-size: 12px; margin: 3px; padding: 3px;">Incremented every time this&nbsp;printer property is updated.</td>
			<td style="font-size: 12px; margin: 3px; padding: 3px;">1</td>
		</tr>
		<tr>
			<td style="font-size: 12px; margin: 3px; padding: 3px;">name</td>
			<td style="font-size: 12px; margin: 3px; padding: 3px;">Char</td>
			<td style="font-size: 12px; margin: 3px; padding: 3px;">Name of the printer</td>
			<td style="font-size: 12px; margin: 3px; padding: 3px;">&nbsp;</td>
		</tr>
		<tr>
			<td style="font-size: 12px; margin: 3px; padding: 3px;">manufacturer</td>
			<td style="font-size: 12px; margin: 3px; padding: 3px;">Char</td>
			<td style="font-size: 12px; margin: 3px; padding: 3px;">Printer manufacturer's name</td>
			<td>&nbsp;</td>
		</tr>
		<tr>
			<td style="font-size: 12px; margin: 3px; padding: 3px;">registration_url</td>
			<td style="font-size: 12px; margin: 3px; padding: 3px;">URL</td>
			<td style="font-size: 12px; margin: 3px; padding: 3px;">URL for registering the printer</td>
			<td style="font-size: 12px; margin: 3px; padding: 3px;">null</td>
		</tr>
		<tr>
			<td style="font-size: 12px; margin: 3px; padding: 3px;">model_number</td>
			<td style="font-size: 12px; margin: 3px; padding: 3px;">Char</td>
			<td style="font-size: 12px; margin: 3px; padding: 3px;">Number of the printer model</td>
			<td style="font-size: 12px; margin: 3px; padding: 3px;">"1.0.0"</td>
		</tr>
		<tr>
			<td style="font-size: 12px; margin: 3px; padding: 3px;">icon_id</td>
			<td style="font-size: 12px; margin: 3px; padding: 3px;">Char</td>
			<td style="font-size: 12px; margin: 3px; padding: 3px;">Relative path and name of the icon file</td>
			<td style="font-size: 12px; margin: 3px; padding: 3px;">"data/my-printers-name.png"</td>
		</tr>
		<tr>
			<td style="font-size: 12px; margin: 3px; padding: 3px;">icon50x50_id</td>
			<td style="font-size: 12px; margin: 3px; padding: 3px;">Char</td>
			<td style="font-size: 12px; margin: 3px; padding: 3px;">Relative path and name of the icon file</td>
			<td style="font-size: 12px; margin: 3px; padding: 3px;"><span>"data/my-printers-name50x50.png"</span></td>
		</tr>
		<tr>
			<td style="font-size: 12px; margin: 3px; padding: 3px;">icon100x100_id</td>
			<td style="font-size: 12px; margin: 3px; padding: 3px;">Char</td>
			<td style="font-size: 12px; margin: 3px; padding: 3px;">Relative path and name of the icon file</td>
			<td style="font-size: 12px; margin: 3px; padding: 3px;">"data/my-printers-name100x100.png"</td>
		</tr>
		<tr>
			<td style="font-size: 12px; margin: 3px; padding: 3px;">technology</td>
			<td style="font-size: 12px; margin: 3px; padding: 3px;">Char</td>
			<td style="font-size: 12px; margin: 3px; padding: 3px;">"FDM" or "DLP"</td>
			<td style="font-size: 12px; margin: 3px; padding: 3px;">"FDM"</td>
		</tr>
		<tr>
			<td style="font-size: 12px; margin: 3px; padding: 3px;">default_material_id</td>
			<td style="font-size: 12px; margin: 3px; padding: 3px;"><a href="https://en.wikipedia.org/wiki/Universally_unique_identifier" target="_blank">UUID</a></td>
			<td style="font-size: 12px; margin: 3px; padding: 3px;">ID of a material in the materials.json file that is located in the same folder as this JSON file.</td>
			<td style="font-size: 12px; margin: 3px; padding: 3px;">"FB67831E-BB63-4C76-BC00-8D84F8F44CB9"</td>
		</tr>
		<tr>
			<td style="font-size: 12px; margin: 3px; padding: 3px;">default_profile_id</td>
			<td style="font-size: 12px; margin: 3px; padding: 3px;"><a href="https://en.wikipedia.org/wiki/Universally_unique_identifier" target="_blank">UUID</a></td>
			<td style="font-size: 12px; margin: 3px; padding: 3px;">ID of a profile in the profiles.json file that is located in the same folder as this JSON file.</td>
			<td style="font-size: 12px; margin: 3px; padding: 3px;">"CF313AC0-FDE6-467A-9E8B-797F0F35E6A3"</td>
		</tr>
		<tr>
			<td style="font-size: 12px; margin: 3px; padding: 3px;">firmware</td>
			<td style="font-size: 12px; margin: 3px; padding: 3px;">JSON</td>
			<td style="font-size: 12px; margin: 3px; padding: 3px;">Printer manufacturer defined JSON containing firmware details.</td>
			<td style="font-size: 12px; margin: 3px; padding: 3px;">{"type": "firmwaretype","version": "1.0.0"}</td>
		</tr>
		<tr>
			<td style="font-size: 12px; margin: 3px; padding: 3px;">build_volume</td>
			<td style="font-size: 12px; margin: 3px; padding: 3px;">JSON</td>
			<td style="font-size: 12px; margin: 3px; padding: 3px;">Information about the build volume of the printer hardware.</td>
			<td>&nbsp;</td>
		</tr>
		<tr>
			<td style="font-size: 12px; margin: 3px; padding: 3px;">build_volume.type</td>
			<td style="font-size: 12px; margin: 3px; padding: 3px;">Char</td>
			<td style="font-size: 12px; margin: 3px; padding: 3px;">"cartesian" (a rectangular prism) or "cylindrical" (a cylinder).</td>
			<td style="font-size: 12px; margin: 3px; padding: 3px;">"Cartesian"</td>
		</tr>
		<tr>
			<td style="font-size: 12px; margin: 3px; padding: 3px;">build_volume.home_position</td>
			<td style="font-size: 12px; margin: 3px; padding: 3px;">Array</td>
			<td style="font-size: 12px; margin: 3px; padding: 3px;">Specifies where to position (in cm) the extruder after the print is complete.</td>
			<td style="font-size: 12px; margin: 3px; padding: 3px;">[1,2,3]</td>
		</tr>
		<tr>
			<td style="font-size: 12px; margin: 3px; padding: 3px;">build_volume.park_position</td>
			<td style="font-size: 12px; margin: 3px; padding: 3px;">Array</td>
			<td style="font-size: 12px; margin: 3px; padding: 3px;">Specifies where to position (in cm) the extruder when the extruder is parked.</td>
			<td style="font-size: 12px; margin: 3px; padding: 3px;">[1,2,3]</td>
		</tr>
		<tr>
			<td style="font-size: 12px; margin: 3px; padding: 3px;">build_volume.bed_size</td>
			<td style="font-size: 12px; margin: 3px; padding: 3px;">Array</td>
			<td style="font-size: 12px; margin: 3px; padding: 3px;">Volume dimensions (usually width,depth,height).</td>
			<td style="font-size: 12px; margin: 3px; padding: 3px;">[1,2,3]</td>
		</tr>
		<tr>
			<td style="font-size: 12px; margin: 3px; padding: 3px;">build_volume.bed_offset</td>
			<td style="font-size: 12px; margin: 3px; padding: 3px;">Array</td>
			<td style="font-size: 12px; margin: 3px; padding: 3px;">Print bed position.</td>
			<td style="font-size: 12px; margin: 3px; padding: 3px;">[1,2,3]</td>
		</tr>
		<tr>
			<td style="font-size: 12px; margin: 3px; padding: 3px;">build_volume.bed_file_id</td>
			<td style="font-size: 12px; margin: 3px; padding: 3px;">Char</td>
			<td style="font-size: 12px; margin: 3px; padding: 3px;">ID and relative path of the <a class="ng-isolate-scope" href="#appendix">print bed model</a>.</td>
			<td style="font-size: 12px; margin: 3px; padding: 3px;">"data/bed_file.zip"</td>
		</tr>
		<tr>
			<td style="font-size: 12px; margin: 3px; padding: 3px;">max_materials</td>
			<td style="font-size: 12px; margin: 3px; padding: 3px;">Numeric</td>
			<td style="font-size: 12px; margin: 3px; padding: 3px;">Maximum number of materials per print job.</td>
			<td style="font-size: 12px; margin: 3px; padding: 3px;">1</td>
		</tr>
		<tr>
			<td style="font-size: 12px; margin: 3px; padding: 3px;">printable</td>
			<td style="font-size: 12px; margin: 3px; padding: 3px;">JSON</td>
			<td style="font-size: 12px; margin: 3px; padding: 3px;">Details of the printable files used by the printer.</td>
			<td style="font-size: 12px; margin: 3px; padding: 3px;"><span>{"content": "application/<g-code format="">",</g-code></span><br />
			<span><g-code format="">"thumbnail": "image/png",<br />
			"extension": "xxx<extension>",<br />
			"generates_supports": false,<br />
			"packager_data": {"icon_file_id": "data/icon_filePrintableIcon.bmp</extension></g-code></span><span>"}}</span></td>
		</tr>
		<tr>
			<td style="font-size: 12px; margin: 3px; padding: 3px;">supported_connections</td>
			<td style="font-size: 12px; margin: 3px; padding: 3px;">Array of JSONs &nbsp;</td>
			<td style="font-size: 12px; margin: 3px; padding: 3px;">Each element contains "type" and "protocol".</td>
			<td style="font-size: 12px; margin: 3px; padding: 3px;">[{"type": "usb","protocol": "<usb protocol="">"}]</usb></td>
		</tr>
		<tr>
			<td style="font-size: 12px; margin: 3px; padding: 3px;">preferred_connection</td>
			<td style="font-size: 12px; margin: 3px; padding: 3px;">Char</td>
			<td style="font-size: 12px; margin: 3px; padding: 3px;">Preferred connection in the supported_connections array.</td>
			<td style="font-size: 12px; margin: 3px; padding: 3px;">"usb"</td>
		</tr>
		<tr>
			<td style="font-size: 12px; margin: 3px; padding: 3px;">software_info</td>
			<td style="font-size: 12px; margin: 3px; padding: 3px;">JSON</td>
			<td style="font-size: 12px; margin: 3px; padding: 3px;">Manufacturer defined data.</td>
			<td style="font-size: 12px; margin: 3px; padding: 3px;">{"name": "<manufacturer name="">","url": "<manufacturer url="">"}</manufacturer></manufacturer></td>
		</tr>
		<tr>
			<td style="font-size: 12px; margin: 3px; padding: 3px;">printer_capabilities</td>
			<td style="font-size: 12px; margin: 3px; padding: 3px;">JSON</td>
			<td style="font-size: 12px; margin: 3px; padding: 3px;">Manufacturer defined data.</td>
			<td style="font-size: 12px; margin: 3px; padding: 3px;">{<br />
			"num_extruders": 1,<br />
			"nozzle_temp_max": 300,<br />
			"nozzle_max_volume_per_sec": 300,<br />
			"nozzle_diameter": 0.04,<br />
			"nozzle_offset": [0, 0, 0],<br />
			"nozzle_retraction_length": 0.1,<br />
			"nozzle_lift_z": 0.05,<br />
			"nozzle_extra_length_on_restart": 0.15,<br />
			"e_speed_max": 2.0,<br />
			"nozzle_min_travel_on_retraction": 0.1,<br />
			"bed_temp_max": 0,<br />
			"xy_speed_max": 20,<br />
			"z_speed_max": 0.01,<br />
			"travel_feed_rate": 10,<br />
			"z_axis_feed_rate": 3<br />
			}</td>
		</tr>
		<tr>
			<td style="font-size: 12px; margin: 3px; padding: 3px;">_files</td>
			<td style="font-size: 12px; margin: 3px; padding: 3px;">Array</td>
			<td style="font-size: 12px; margin: 3px; padding: 3px;">Array of the fields in this JSON which reference external&nbsp;files.</td>
			<td style="font-size: 12px; margin: 3px; padding: 3px;">[<br />
			"icon_id",<br />
			"icon50x50_id",<br />
			"icon100x100_id",<br />
			"build_volume.bed_file_id",<br />
			"printable.packager_data.icon_file_id"<br />
			]</td>
		</tr>
	</tbody>
</table>