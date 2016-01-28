# Workshop 4 - Creating Services

#### Create blank mBaaS Service

- Goto Services & APIs
- Select 'Provision an mBaaS service'
- Select 'New mBaaS Service'
- Create this:
- When Completed - Select 'Finish'

- On Details screen select <Current Host> (opens in browser)
e.g. https://sales-eu-ecb0rnr4gndbrblijx0dyyhr-dev.ac.gen.ric.feedhenry.com/
To ensure it's running & add /hello to the end (to see the 'hello' route).
https://sales-eu-ecb0rnr4gndbrblijx0dyyhr-dev.ac.gen.ric.feedhenry.com/hello

#### Adding some route logic & 3rd Party Service

- Goto Editor:
	- In application.js
	- Add a new custom route (so it looks like the following - ONLY HAVE 1 ‘HELLO’ line)
	
```
app.use('/hello', require('./lib/hello.js')());
app.use('/weather', require('./lib/weather.js')());
```
- SAVE this change.

- In 'LIB' directory - create this weather.js file (file->new file)

**
Note: We're going to use a weather service @ http://openweathermap.org/api
Scroll to: 'API Documentation' & select '  How to get (JSON, XML, HTML)
Choose JSON : api.openweathermap.org/data/2.5/weather?q=London,uk
**

- Copy the hello.js file to weather.js & change the code to use the weather service provided
* Note : Don’t use London,UK - get these as parameters from your client app (req.body)
* Note: Mount the .all route (as to having separate POST/GET (Example below)
* Note: Use the Request module

- If you don’t feel like writing the code that’s ok, we’ve added the code here for you :)

*weather.js*

```
var express = require('express');
var bodyParser = require('body-parser');
var cors = require('cors');
var request = require('request');

function weatherRoute() {
  var weather = new express.Router();
  weather.use(cors());
  weather.use(bodyParser());

  // GET REST endpoint - query params may or may not be populated
  weather.all('/', function(req, res) {
    var city = req.body.city  || req.query.city,
    country = req.body.country || req.query.country;
    request.get({
      url  : "http://api.openweathermap.org/data/2.5/weather?q=" + city + "," + country+”&APPID=1152e19c7865d732e3c8cd51bbdca174”,
      json : true
    }, function(error, response, body){
      if (error){
        return res.status(500).json(error);
      }
      return res.json(body);
    });
  });

  return weather;
}

module.exports = weatherRoute;
```


- SAVE this file
- DEPLOY!!
- Test in the browser select <CURRENTHOST>/weather to see this route (add params as needed <CURRENTHOST>/weather?city=Paris&country=France etc.)
- *Any issues make the Service Global , Save & Deploy.*


#### Make Service Discoverable/Docs
- Goto Docs
- See the Hello Docs
- Try it out (Test the endpoint, change “world” then submit etc.)
- Goto Editor -> Open README.md
- Alter to Document (API Blueprint) for the Weather API (overwrite whats there with the following)

*README.md*

```
# FeedHenry Hello World MBaaS Server

This is a blank 'hello world' FeedHenry MBaaS. Use it as a starting point for building your APIs. 

# Group Weather API

# weather [/weather]

'Weather' endpoint.

## weather [POST] 

'Weather' post endpoint.

+ Request (application/json)
    + Body
            {
              "city": "Orlando",
              "country" : "USA"
            }

+ Response 200 (application/json)
    + Body
            {
              "msg": "Some weather data"
            }

```
- SAVE
- Go to Docs & See the Weather APIs
- ‘Try It’… to see it in action.

- We’ve now got a reusable service!! :)

#### Next Steps
- Create a project that uses your service
- Associate it with your project(s).
-  Use the $fh.service snippet (for using this service) - http://docs.feedhenry.com/v3/api/cloud_api.html#cloud_api-_fh_service
- Add this to Cloud App (homework - See FeedHenry Docs page ! :) )





