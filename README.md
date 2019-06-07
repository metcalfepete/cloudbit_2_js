# cloudbit_2_js
Javascript/HTML interface to littleBits Cloudbit REST API

This project has some Javascript interface examples to the littleBits' Cloudbit REST API.

The examples are:
* __js_status.htm__ - show the Cloudbit device status
* __js_in.htm__ - read Cloudit input
* __js_out2.htm__ - send an output and the duration of the output to a Cloudbit
* __js_gauge.htm__ - show a Cloudbit input on a real time gauge
* __js_chart2.htm__ - show a Cloudbit input on a real time chart


# Introduction

To get started creating littleBits Cloud API applications, you will need to get:
1. littleBits Device ID – this is a unique identifier for each littleBits Cloud bit device
2. AccessToken – this is an authorization code that needs to be passed with all HTTP requests

To get find you Device ID and AccessToken log into [http://littlebits.cc/](http://littlebits.cc/) and on the top right side of the screen select the “Cloud Control” menu item. Then select the “Settings” icon at the bottom of the page. 

At the bottom of the “Settings” page the Device ID and AccessToken will be shown.

![alt text](bit_info.bmp)

There are a number of different calls that are available with the littleBits Cloud HTTP API. Unlike a standard call to a Web page, the Cloud HTTP API requires some extra parameters. These extra parameter are included in the header definition of the HTTP request. The most important header item is the littleBits authorization token. A version item can also be defined through the “Accept” header item.

The Cloudbit API reference can be found at : [http://developers.littlebitscloud.cc/#devices](http://developers.littlebitscloud.cc/#devices)


# js_status.htm - show the Cloudbit device status
![alt text](js_status.png)

This is a simple "one time read" of the name and status of a CloudBit. The basic Javascript code to setup the REST API get and then to parse the response is below.

```javascript
var xhttp = new XMLHttpRequest();
// update with your token information
var authtoken = "4f3830b44e1d4b27xxx";
var theURL =  "https://api-http.littlebitscloud.cc/devices/";
xhttp.open("GET", theURL, true);
xhttp.setRequestHeader("Accept","application/vnd.littlebits.v2+json");
xhttp.setRequestHeader("Authorization", "Bearer " + authtoken);

xhttp.onreadystatechange = function() {
	
	if (xhttp.readyState == 4 ) { // when the response is complete get the data
		// remove leading "[" and trailing ']'
		var theresponse = xhttp.responseText.substring(1, xhttp.responseText.length -1);
		var lb_data = JSON.parse( theresponse );	
		
		document.getElementById('LB_label').innerText =  lb_data.label;
		document.getElementById('is_connected').innerHTML = lb_data.is_connected;
	}
}

xhttp.send();
```

# js_in.htm - read Cloudit input
![alt text](js_in.png)

The inputs from the CloudBit API are streamed to the client. This can be a little messy if the client application is only expecting a single value. Using the statement: _xhttp.readyState == 3_ will allow the application to catch the first stream of data.

```html
<!DOCTYPE html>
<html>
<head>
<title>littleBits Get Input</title>
<script>
var theinput = 0;

function get_input() {
// update with your deviceid and authtoken
  var deviceid = "00e04c0379bb";
  var authtoken = "4f3830b44e1d4b27xxxx";
  var theurl = "https://api-http.littlebitscloud.cc/devices/";
  var xhttp = new XMLHttpRequest();
  xhttp.onreadystatechange = function() {

    if (xhttp.readyState == 3 ) {
	  	var datapackage = xhttp.responseText.split("\n\ndata:");
		var lb_data = JSON.parse( datapackage[1] );		 
		document.getElementById("thevalue").innerText =  lb_data.percent;
		xhttp.open("GET","",true);
		xhttp.send();
    }
  }
  xhttp.open("GET", theurl + deviceid + "/input", true);
  xhttp.setRequestHeader("Accept","application/vnd.littlebits.v2+json");
  xhttp.setRequestHeader("Authorization", "Bearer " + authtoken);
  xhttp.send();
}
</script>
</head>
<body>

<h1 id='title'>littleBit Get Input</h1>
The value : <font id="thevalue"> XXXX </font>
<button type="button" onclick="get_input()">Request data</button>
<br>

</body>
</html>
```

# js_out2.htm - send an output and the duration of the output to a Cloudbit
![alt text](in_out2.png)

This example will output the value (0-100) and the pulse duration in milliseconds. For a constant output use a duration of -1. Below is the example code.

```html
<!DOCTYPE html>
<html>
<body>
<h2>CloudBit Output Test</h2>
<button type="button" onclick="sendoutput()">Send Output</button>
<br>
<pre>
Output Time (ms): <input type="text" id="duration" value="-1"/> (constant = -1)</br>
Output Value	: <input type="text" id="thevalue" value="80"/> (percent 0-100)</br> 
</pre>
<p id="demo"></p>
<script>

function sendoutput() {
	var xhttp = new XMLHttpRequest();

	xhttp.open("POST", "https://api-http.littlebitscloud.cc/devices/00e04c0379bb/output?", true);
	xhttp.setRequestHeader("Accept","application/vnd.littlebits.v2+json");
	// change to your Auth Token
	xhttp.setRequestHeader("Authorization", "Bearer 4f3830b44e1d4b27xxxx");
	xhttp.setRequestHeader("Content-Type","application/json");

	var duration = document.getElementById("duration").value;
	var thevalue = document.getElementById("thevalue").value;

	var params = JSON.stringify({duration_ms: duration,percent: thevalue});

	xhttp.onreadystatechange = function() {
		document.getElementById("demo").innerHTML = "Result=" +xhttp.responseText;
	}

	xhttp.send(params);
}
</script>

</body>
</html>
```
# js_gauge.htm - show an input on a real time gauge
![alt text](js_gauge.png)

The gauge example is uses:
* AJAX to get updates on the Cloudbits input value
* Google Charts for the gauge component

# js_chart2.htm - show an input on a real time chart
![alt text](lb_chart2.png)

This example uses smoothie charts [http://smoothiecharts.org/](http://smoothiecharts.org/) for the real time charts
