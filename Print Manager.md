<h1>Print Manager</h1>

<div>
<p class="ng-scope">Print Manager is a utility program installed by <a href="/developers/reference/desktop-applications/print-studio">Print Studio</a>&nbsp;that&nbsp;manages its 3D printer connectivity.&nbsp;</p>

<p class="ng-scope">The Print Manager has a console you can load from your app&nbsp;and&nbsp;runs a local version of the Spark API that can be called without requiring an internet connection.</p>

<p class="ng-scope">If you have Spark-compatible 3D printers connected by USB or over a network with Bonjour (on Macs), Print Manager will detect their presence and Print Studio will provide you with options to send your file to a printer.&nbsp;</p>

<p class="ng-scope">If you are a 3D printer manufacturer<span>&nbsp;and want&nbsp;to integrate a 3D&nbsp;printer with Spark, please refer to <a href="/developers/reference/printer-manufacturers/integrate-your-printer/integrate-your-printer-model" title="Printer Manufacturers">this guide</a>.</span></p>

<hr class="ng-scope" />
<p class="ng-scope">&nbsp;</p>

<p class="ng-scope"><strong>Benefits of Print Manager Integration:</strong></p>

<ol class="ng-scope">
	<li>Can be opened&nbsp;directly from your app.</li>
	<li>Automatically detects and connects with local 3D printers.</li>
	<li>Creates and manages 3D print-jobs.</li>
	<li>Monitors print job execution.</li>
	<li>Can be used to call Print APIs locally.</li>
	<li>Keeps a log of its activity. The location of the log can be set in the advanced options.</li>
</ol>

<h3 class="ng-scope">Loading&nbsp;the Print Manager Console</h3>

<p class="ng-scope" style="padding-left: 30px;"><strong>To open</strong> the Print Manager Console redirect to<em><strong>&nbsp;<a href="http://localhost:9998/console" target="_blank">http://localhost:9998/console</a></strong></em>. &nbsp;This opens the Print Manager's <em>Status</em> screen.</p>

<ul class="ng-scope">
	<li><strong>Manage your printers</strong> - &nbsp;Click the <em>Edit</em> button&nbsp;(top right),&nbsp;this opens the Manage Printers&nbsp;screen.

	<ul>
		<li>Adds a new printer.</li>
		<li>Sets the IP address of the printer.</li>
	</ul>
	</li>
	<li><strong>Set advanced options</strong> - Click the <em>Settings</em> button (top left), this opens the Advanced Options screen.
	<ul>
		<li>Sets the port that the console runs from (the default is 9998).</li>
		<li>Sets the log file name and location.</li>
	</ul>
	</li>
</ul>

<h3 class="ng-scope">&nbsp;<img alt="x1" class="alignnone size-full wp-image-486" height="373" src="https://s3.amazonaws.com/spark-dev-portal-prd/blog-alpha-content/uploads/2015/05/x16.png" width="359" /></h3>

<h3 class="ng-scope">Using Print Manager to call Print APIs</h3>

<p class="ng-scope" style="padding-left: 30px;">Print Manager&nbsp;service/daemon implements our API server on your desktop. It can be used to call Spark Print APIs directly, without loading Print Studio or connecting to the cloud; simply replace the <em>URL prefix</em> with <strong>http://localhost:9998</strong>.</p>

<p class="ng-scope" style="padding-left: 30px;"><strong><code>GET <em>https://sandbox.spark.autodesk.com/api/v1/print/printers</em></code></strong><br />
is replaced with<br />
<strong><code><em>GET http://localhost:9998/print/printers</em></code></strong></p>

<p class="ng-scope" style="padding-left: 30px;">Print Manager runs&nbsp;a version of the Drive API's <strong>file upload </strong>and<strong> file download</strong> on your local machine, but does not provide access to cloud data or cloud APIs such as the&nbsp;<em>Drive API</em> or <em>Service Bureau API</em>. Assets stored on the Spark servers will require a regular API call.</p>

<p class="ng-scope" style="padding-left: 30px;">Printer Registration and Print Job Creation for local printers use different parameters from cloud printers.</p>

<p class="ng-scope" style="padding-left: 30px;">See the&nbsp;<a href="https://spark.autodesk.com/developers/reference/print" target="_blank">Print API</a> documentation for a list of commands.&nbsp;</p>

<p class="ng-scope">&nbsp;</p>

<p class="ng-scope">&nbsp;</p>

<h1 class="ng-scope">&nbsp;</h1>
</div>
