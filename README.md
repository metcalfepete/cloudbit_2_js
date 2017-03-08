# cloudbit_2_js
Javascript/HTML interface to littleBits Cloudbit REST API

This project address how to create Javascript and Web pages to interface with the littleBits' Cloudbit REST API, with examples of:
* monitoring Cloudbit devices
* reading/charting and trending Cloudbit inputs
* setting Cloudbit outputs

The Cloudbit API reference can be found at : [http://developers.littlebitscloud.cc/#devices](http://developers.littlebitscloud.cc/#devices)

For testing it is recommended that **curl** be loaded.

#Introduction

There are a number of different calls that are available with the littleBits Cloud HTTP API. Unlike a standard call to a Web page, the Cloud HTTP API requires some extra parameters. These extra parameter are included in the header definition of the HTTP request. The most important header item is the littleBits authorization token. A version item can also be defined through the “Accept” header item.

To get started creating littleBits Cloud API applications, you will need to get:
1. littleBits Device ID – this is a unique identifier for each littleBits Cloud bit device
2. AccessToken – this is an authorization code that needs to be passed with all HTTP requests

To get find you Device ID and AccessToken log into [http://littlebits.cc/](http://littlebits.cc/) and on the top right side of the screen select the “Cloud Control” menu item. Then select the “Settings” icon at the bottom of the page. 

At the bottom of the “Settings” page the Device ID and AccessToken will be shown.


#Device Monitoring

The GET Devices call is useful for seeing the status of your Cloud Bits devices. 

The curl syntax would be:

```bash
curl -i -XGET -H "Authorization: Bearer TOKEN" -H "Accept: application/vnd.littlebits.v2+json" https://api-http.littlebitscloud.cc/devices/DEVICE_ID
```

A curl example with a response would be:
```bash
curl -i -XGET -H "Authorization: Bearer 4f3830b44e1d4b2xxxxxxxxxxxb493f06750b968cff5d45331c75006025fa0dc9" -H "Accept: application/vnd.littlebits.v2+json" -H "Content-Type: application/json"  https://api-http.littlebitscloud.cc/devices/00e04c0379bb
HTTP/1.1 200 OK
Date: Wed, 08 Mar 2017 13:23:47 GMT
Server: littleBits Cloud Platform
Access-Control-Allow-Origin: *
Content-Type: application/json;charset=UTF-8
Transfer-Encoding: chunked

{"label":"twinbit","id":"00e04c0379bb","subscriptions":[],"subscribers":[],"user_id":118217,"is_connected":true,"input_interval_ms":200}
```
A simple Javascript/HTML example to get littleBits device information would be:
```html
<!DOCTYPE html>
<html>
<body>
<h1 id='title'>Cloudbit Status Monitor</h1>
<h2>
Label : <font color='red' id='LB_label'></font><br>
Is Connected : <font color='red' id='is_connected'></font><br>
</h2>
 
<script>

var xhttp = new XMLHttpRequest();

var authtoken = "4f3830b44e1d4b2xxxxxxxxxxxxxx750b968cff5d45331c75006025fa0dc9";
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

</script>

</body>
</html>
```

The Web page should look something like:







