# apns2-spdy
[![Known Vulnerabilities](https://snyk.io/test/npm/apns2-spdy/badge.svg)](https://snyk.io/test/npm/apns2-spdy)

apns2-spdy

Documentation and use of APIs with examples is exactly same as apns2
https://www.npmjs.com/package/apns2

The http2 client used in the implementation is node-spdy https://github.com/indutny/node-spdy instead of node-http2 used in apns2.
In personal point of view node-spdy is way stable than node-http2 as far as h2 implementation is concerned.

# Sample Code (in Koa)
```javascript
'use strict';

var koa=require("koa");
var app=koa();
const APNS = require('apns2-spdy');
const fs =require ('fs');

app.use(function *(){
  let client = new APNS({
    host: 'api.development.push.apple.com',//or api.push.apple.com
    keyId:'XXXXXXXXXX',//10 chararcter key id ,common for both dev and prod environments
    team: `YYYYYYYYYY`,//10 character team id,common for both dev and prod environments
    signingKey: fs.readFileSync('./APNSAuthKey_XXXXXXXXXX.p8'),
    defaultTopic:'com.xxxx.ios.voip'//use voip for a voip notification
    });

  const Notification = APNS.Notification;
  let notification = new Notification('aaaaabbbbcddddddccccccnnnnbbbbvvvvvggggg1111122222223333', { // device notification id //or device token
    aps: {
      expiry:12345,
      alert:'test notification',
      contentAvailable:1,
      category:'some Category'
    },
    data:{
      data:'data is test data...'
    }
  });

  client.send(notification).then((res) => {
    console.log("call notification sent"); //on success this message will be displayed in console
  }).catch(err => {
    console.log("we have an error ", err); //on error this message will be displayed in console with error details
  });

})
app.listen(8080);
```
Just open http://localhost:8080 in the browser
