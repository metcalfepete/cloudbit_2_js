# cloudbit_2_js
Javascript/HTML interface to littleBits Cloudbit REST API

This project has some Javascript interface examples to the littleBits' Cloudbit REST API.

The examples are:
* __js_status.htm__ - show the Cloudbit device status
* __js_in.htm__ - read Cloudit input
* __js_out2.htm__ - send an output and the duration of the output to a Cloudbit
* __js_gauge.htm__ - show a Cloudbit input on a real time gauge
* __js_chart2.htm__ - show a Cloudbit input on a real time chart


#Introduction

To get started creating littleBits Cloud API applications, you will need to get:
1. littleBits Device ID – this is a unique identifier for each littleBits Cloud bit device
2. AccessToken – this is an authorization code that needs to be passed with all HTTP requests

To get find you Device ID and AccessToken log into [http://littlebits.cc/](http://littlebits.cc/) and on the top right side of the screen select the “Cloud Control” menu item. Then select the “Settings” icon at the bottom of the page. 

At the bottom of the “Settings” page the Device ID and AccessToken will be shown.
![alt text](bit_info.bmp)

There are a number of different calls that are available with the littleBits Cloud HTTP API. Unlike a standard call to a Web page, the Cloud HTTP API requires some extra parameters. These extra parameter are included in the header definition of the HTTP request. The most important header item is the littleBits authorization token. A version item can also be defined through the “Accept” header item.

The Cloudbit API reference can be found at : [http://developers.littlebitscloud.cc/#devices](http://developers.littlebitscloud.cc/#devices)

#js_status.htm - show the Cloudbit device status
![alt text](js_status.png)

#js_in.htm - read Cloudit input
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








