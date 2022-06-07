# Cam Solutions <!-- omit in toc -->

Whether you run a cam site or a service for the camming industry, our Cam Solutions will let you easily integrate our best-in-class technology into your platform. Add tip-activated vibrations to your platform and let your users join the 90,000+ models using Lovense on cam.

# Contents  <!-- omit in toc -->

<!-- TOC -->


- [Cam Extension for Chrome](#cam-extension-for-chrome)
	- [Configure the developer dashboard](#configure-the-developer-dashboard)
	- [Integration](#integration)
	- [Methods and Events](#methods-and-events)
		- [Methods](#methods)
		- [Events](#events)
- [Cam Kit for Web](#cam-kit-for-web)
	- [Configure your developer dashboard](#configure-your-developer-dashboard)
	- [Generate an mToken for your model](#generate-an-mtoken-for-your-model)
	- [Model configures their settings](#model-configures-their-settings)
	- [Broadcasting events and methods](#broadcasting-events-and-methods)
		- [Broadcast events](#broadcast-events)
		- [Broadcast methods](#broadcast-methods)
- [Display Panel (optional)](#display-panel-optional)
	- [Turn on Display Panel on Lovense Developer Dashboard](#turn-on-display-panel-on-lovense-developer-dashboard)
	- [Import the tipper.js to your tipper's page](#import-the-tipperjs-to-your-tippers-page)
	- [Initialize](#initialize)
	- [Data Forwarding](#data-forwarding)
- [Custom Cam Solution](#custom-cam-solution)
	- [Basic API](#basic-api)
		- [Configure developer dashboard](#configure-developer-dashboard)
		- [Connect to Lovense toy(s)](#connect-to-lovense-toys)
		- [Command the toy(s)](#command-the-toys)
			- [By local application](#by-local-application)
			- [By server application](#by-server-application)
	- [Basic JS API](#basic-js-api)
		- [Set your callback URL](#set-your-callback-url)
		- [Import the LAN.JS to your user's page](#import-the-lanjs-to-your-users-page)
		- [Handle the callback data from Lovense Connect](#handle-the-callback-data-from-lovense-connect)
		- [Command the toy(s)](#command-the-toys)
		- [Other methods](#other-methods)
		
		
		
# Cam Extension for Chrome
The Cam Extension for Chrome is the best solution for cam models to stream with Lovense toys, featuring:

- Highly customizable vibration levels and patterns
- Lovense widget for quick access to settings while streaming
- Video overlay support for OBS, Streamster, SplitCam
- Games and tools to increase interactivity and earn more tips
- Frequent updates, improvements, and new features

## Configure the developer dashboard

Field	|Description
:-------|:----------
Website Name|	The name that will be displayed in our cam site list (in the Cam Extension). Contact us if you want to change it.
Website URL	|Your home page URL.
Model Broadcasting Page Prefix	|The URL prefix of your model broadcasting page.
Status	|The status of your website.

Go to the [developer dashboard](https://www.lovense.com/user/developer/info) and fill in your settings.

Status Meaning:

Pending - The website is being tested. Use `test:{Website Name}` name in order to add your website to the Cam Extension during the testing phase.

Active - The website is live and public. All models can see your cam site in the Cam Extension

## Integration

**Import the Javascript file**<br>
Import the Javascript file to your model’s broadcasting page. This Javascript will declare a global variable CamExtension on the page.
```
<script src="https://api.lovense.com/cam-extension/static/js-sdk/broadcast.js"></script>
```
**Initialize the instance object**<br>
Initialize a CamExtension instance on the model’s live page.
```
/**
 * @param {string} websiteName this is the Website Name shown in the developer dashboard
 * @param {string} modelName this is the model's name
 * @returns {object} CamExtension instance object
 */
const camExtension = new CamExtension(websiteName, modelName)
```
**Listen for ready event**<br>
Listen for the ready event, which will be called after successful initialization. You can communicate with the Cam Extension after this event is triggered.
```
/**
 * readyCallback
 * @param {object} ce CamExtension instance object
 */
const readyCallback = function (ce) {
  // Handle the CamExtension instance object
  // e.g. await ce.getCamVersion()
}

camExtension.on("ready", readyCallback)
```


## Methods and Events

### **Methods**
- receiveTip

Call this method when the model receives a tip. The Cam Extension will trigger a response in the toy according to these parameters:
```
/**
 * receiveTip
 * @param {number} amount  tip amount that the model receives
 * @param {string} tipperName this is the tipper’s Screen Name
 */
camExtension.receiveTip(amount, tipperName)
```

- receiveMessage

Call this method when the model receives a message in the chat room.
```
/**
 * receiveMessage
 * @param {string} userName  the sender’s Screen Name
 * @param {string} content the message just sent by the sender
 */
camExtension.receiveMessage(userName, content)
```
- getSettings

Get the model's Lovense Settings.
```
/**
 * getSettings
 * @returns {object} model's Lovense Settings
 */
const data = await camExtension.getSettings()
// data = {
//  levels: {
//    level1: {
//     min: 1,
//     max: 9,
//     time: 2,
//     rLevel: 0,
//     vLevel: 0,
//    },
//    level2: {...}
//    level3: {...}
//  },
//  special: {
//    earthquake: {
//      time: "22",
//      token: "120",
//    },
//    fireworks: (...),
//    giveControl: (...),
//    pause: (...),
//    pulse: (...),
//    random: (...),
//    twowaves: (...),
//    wave: (...),
//  }
// }
```

- getToyStatus

Get the model's Lovense Toys status.
```
/**
 * getToyStatus
 * @returns {array} model's Lovense Toys status.
 */
const data = await camExtension.getToyStatus()
// data = [
//  {
//    id: "l58f167da065",
//    name: "",
//    type: "lush",
//    status: "on"
//  },
//  {...}
// ]
```

- getCamVersion

Get the Cam Extension version.
```
/**
 * getCamVersion
 * @returns {string} Cam Extension version
 */
const data = await camExtension.getCamVersion()
// data = "30.4.4"
```

### **Events**

You can listen for certain events to get the latest real-time data

- postMessage

We will trigger this event when we need to send a message to you or the chat room, and you will need to help us handle it accordingly.
```
camExtension.on("postMessage", (message) => {
  // Process the message to be sent
  // Send the message to chat room
  // e.g. message = "My LOVENSE Lush is now reacting to john's tip. It will stop after 5 sec!"
})
```

- toyStatusChange

We will trigger this event when the toy status changes.
```
camExtension.on("toyStatusChange", (data) => {
  // Handle toy information data
  // data = [{
  //  id: "d6c35fe83348",
  //  name: "toy's name",
  //  type: "lush",
  //  status: "on",
  //  version: "",
  //  battery: "80"
  // }]
})
```

- tipQueueChange

We will trigger this event when the tip queue updates.
```
camExtension.on("tipQueueChange", (data) => {
  //  handle queue information data
  //  data = {
  //   running: [
  //    {
  //      amount: 100,
  //      tipperName: "john",
  //      time: 20,
  //      module: "Special Command",
  //      cParameter: {},
  //      level: "",
  //      specialType: "earthquake",
  //      modelName: "coco",
  //      reactToys: [,
  //        toyId: "d6c35fe83348",
  //        specialType: "earthquake",
  //        status: 1,
  //        toyType: "lush",
  //      ]
  //    }
  //   ],
  //   queue: [...],
  //   waiting: [...]
  //  }
})
```

- settingsChange

We will trigger this event when the model’s Lovense Settings are updated.
```
camExtension.on("settingsChange", (data) => {
  //  handle configuration information data
  // data = {
  //  levels: {
  //    level1: {
  //     min: 1,
  //     max: 9,
  //     time: 2,
  //     rLevel: 0,
  //     vLevel: 0,
  //    },
  //    level2: {...}
  //    level3: {...}
  //  },
  //  special: {
  //    earthquake: {
  //      enable: true,
  //      time: "20",
  //      token: "100",
  //    },
  //    fireworks: (
  //      enable: true,
  //      time: "22",
  //      token: "120",
  //    ),
  //    giveControl: (
  //      enable: false,
  //      time: "",
  //      tokensBegin: "",
  //      tokensEnd: "",
  //    ),
  //    randomTime: (
  //      enable: true,
  //      tokens: "38",
  //      minTime: 10,
  //      maxTime: 50,
  //      level: 5,
  //    ),
  //    pause: (...),
  //    pulse: (...),
  //    random: (...),
  //    wave: (...),
  //  }
  // }
})
```


# Cam Kit for Web

## Configure your developer dashboard

Go to the [developer dashboard](https://www.lovense.com/user/developer/info)
Get your dToken (Developer Token).
Click "Cam Kit Developer settings (v2.0)", configure your settings.

## Generate an mToken for your model

Get a unique token for your model. You only need to do this once for each model.

**API URL**: `https://api.lovense.com/api/cam/model/getToken`

> Notice: For security reasons, the developer token should be always used on the server side. You should never use it in your JS code from the client side.

**HTTP Request**: POST

**Content Type**: application/x-www-form-urlencoded; charset=UTF-8

**Parameters:**

Name	|Description
:-------|:------------
dToken	|Developer Token
mInfo	|Model's Information (UTF-8 encoded JSON)

**Request example:**
```
// content-type: application/x-www-form-urlencoded; charset=UTF-8
{
  "dToken": "xxxx", // developer token
  "mInfo": "{'mId':'b799e042b','mName':'test123'}" //
}
```

**Return:**
```
{
  "result": true, // true or false
  "message": "SuccessFully",
  "code": 200, // 200: OK 401: Invalid token 402: Invalid user information 505: Other Errors
  "data": {
    "mId": "b799e042b",
    "mToken": "95116bd2af44b753da2a1e4cd55cf965" // got mToken
  }
}
```
Name	|Description
:-------|:-----------
result	|true if successful, false if failed
message	|Request result
data	|Detailed description
code	|200: OK, 401: Invalid token, 402: Invalid user information, 505: Other Errors


## Model configures their settings

Display the Lovense settings page on the model's side. You can put this page in a new window or an iframe.

After you modify your settings, the new settings will take around 10 seconds to take effect.

**Settings page**: `https://api.lovense.com/api/cam/model/v2/setting?mToken={mToken}`

**Sample code:**
```
lovense.settingPage = window.open(
  "https://api.lovense.com/api/cam/model/v2/setting?mToken={mToken}"
)
```
or
```
<iframe
  id="lovense-setting"
  width="100%"
  height="1500px"
  scrolling="no"
  src="https://api.lovense.com/api/cam/model/v2/setting?mToken={mToken}"
></iframe>
```

## Broadcasting events and methods

Include the model.js on your broadcasting page.
```
<script src="https://api.lovense.com/api/cam/model/v2/model.js?mToken={mToken}"></script>
```
Call `lovense.addMessageListener(callback)` after the live page is loaded, and handle these types of messages in the callback function:

### **Broadcast Events**

- message
```
lovense.addMessageListener(function (data) {
  // data = {
  //  type: "message",
  //  detail: "some customized messages to be sent to the public chat room."
  //  e.g. detail = "My LOVENSE Lush is now reacting to john's tip. It will stop after 5 sec!"
  //}
})
```
- settings
```
lovense.addMessageListener(function (data) {
  // Handle settings information
  // data = {
  //  type: "settings",
  //  detail: {
  //   levels: {
  //     level1: {
  //      min: "1",
  //      max: "9",
  //      time: "2",
  //      rLevel: 0,
  //      vLevel: 0,
  //     },
  //     level2: {...}
  //     level3: {...}
  //   },
  //   special: {
  //     earthquake: {
  //       time: "22",
  //       token: "120",
  //     },
  //     fireworks: (...),
  //     giveControl: (...),
  //     pause: (...),
  //     pulse: (...),
  //     random: (...),
  //     twowaves: (...),
  //     wave: (...),
  //   }
  //  }
  // }
})
```
- toy
```
lovense.addMessageListener(function (data) {
  // Handle toy information data
  // data = {
  //  type: "toy",
  //  status: "on"
  //  detail: [
  //   {
  //     id: "l58f167da065",
  //     name: "",
  //     type: "lush",
  //     status: "on"
  //   }
  //  ]
  // }
})
```
- tipQueueStatus
```
lovense.addMessageListener(function (data) {
  // handle queue information data
  // data = {
  //  type: "tipQueueStatus",
  //  detail: {
  //   running: [
  //    {
  //      amount: 20,
  //      tipperName: "john",
  //      time: 30,
  //      cParameter: {},
  //      level: 5,
  //      module: "Basic Level"
  //      specialType: undefined,
  //      name: "coco",
  //      reactToys: [,
  //        toyId: "d6c35fe83348",
  //        specialType: "earthquake",
  //        status: 1,
  //        toyType: "lush",
  //        vibrate: 20,
  //        rotate: 0,
  //        airpump: 0,
  //      ]
  //    }
  //   ],
  //   queue: [
  //    {
  //      amount: 20,
  //      tipperName: "john",
  //    }
  //   ],
  //   waiting: [...]
  //  }
  // }
})
```
- tipRunning
```
lovense.addMessageListener(function (data) {
  // handle running tip information
  // data = {
  //  type: "tipRunning",
  //  detail: {
  //   running: [
  //    {
  //      amount: 20,
  //      tipperName: "john",
  //      time: 30,
  //      cParameter: {},
  //      level: 5,
  //      module: "Basic Level"
  //      specialType: undefined,
  //      name: "coco",
  //      reactToys: [,
  //        toyId: "d6c35fe83348",
  //        specialType: "earthquake",
  //        status: 1,
  //        toyType: "lush",
  //        vibrate: 20,
  //        rotate: 0,
  //        airpump: 0,
  //      ]
  //    }
  //   ]
  //  }
  // }
})
```
### **Broadcast Methods**

- initCamApi

If your website is a single page application, when the model enters the broadcasting page call initCamApi.
```
/**
 * initCamApi
 * @param {string} mToken  model token
 */
lovense.initCamApi(mToken)
```
- destroyCamApi

When the model ends the live broadcast or signs out, call destroyCamApi.
```
lovense.destroyCamApi()
```
- receiveTip

Once a tip is received, call `Lovense.receiveTip(cname,amount)`. Toys will respond according to the settings.
```
/**
 * receiveTip
 * @param {string} cname  this is the tipper’s Screen Name
 * @param {number} amount  token amount
 */
lovense.receiveTip(cname, amount)
// e.g. lovense.receiveTip('cname', 1)
```
- getToys

Use `getToys()` to get the toy info of models
```
/**
 * getToys
 * @returns {array} toy information
 */
lovense.getToys()
```
Return:
```
[
  {
    "id": "XXXXXXXXXXXX",
    "name": "",
    "status": "on",
    "type": "lush"
  }
]
```
- getSettings

Use getSettings to get the settings info of models
```
/**
 * getSettings
 * @returns {object} settings information
 */
lovense.getSettings()
```
Return:
```
{
    levels: {
      level1: {
        max: 10, // tip level range maximum
        min: 3,  // tip level range minimum
        rLevel: 0, // rotation level
        time: 3, // duration (seconds)
        vLevel: 5  // vibration level
      },
      ...
    },
    special: {
      clear: {
        token: 20 // number of tokens
      },
      earthquake: (...),
      fireworks: (...),
      giveControl: (...),
      pause: (...),
      pulse: (...),
      random: (...),
      twowaves: (...),
      wave: (...)
    }
}
```


# Display Panel (optional)

Display Panel (optional)
The Lovense Display Panel makes the show much more interactive compared to simple text based tip activation. We currently provide 2 panels for your tippers/viewers.

- Control Panel - This enables a new tip menu item called Give Control. It automatically generates a control link for the tipper when they tip for this item, allowing models to give direct control of their toys to tippers much more easily.
<img src="https://developer.lovense.com/assets/img/displaypanel-control.17e395c3.png" />

- Tip Panel - Show the model's tip menu directly on the viewer's page.
<img src="https://developer.lovense.com/assets/img/displaypanel-panel.e73c416b.png" />

## Turn on Display Panel on Lovense Developer Dashboard
<img src="https://developer.lovense.com/assets/img/displaypanel-setting.860f1600.png" />

You will get a default AES KEY and AES IV that are used for encryption. Use them to encrypt the model name and the tipper name.

## Import the tipper.js to your tipper's page
```
<script src="https://api.lovense.com/api/cam/tipper/v2/tipper.js"></script>
```
## Initialize
```
Lovense.init(platform, modelKey, tipperKey)
```
**Parameters**

Name	|Description	|Required
:-------|:--------------|:-------
platform	|Your Website Name (shown in the Developer Dashboard)	|yes
modelKey	|The model name encrypted on your server side using the AES KEY and AES IV. Do not expose your Key/IV in your JS code. (There is a demo on how to encrypt text using AES in JAVA below)	|yes
tipperKey	|The tipper name encrypted using the AES KEY and AES IV (The tipper name should be the Display Name showing in the public broadcasting room)	|yes

> ⚠️ Display Panels are only available when the model is using Cam Extension version 30.0.8+

If your website is a single page application, when the tipper leaves the model's room, call:

```
Lovense.destroy()
```

Here is the demo on how to encrypt text using AES in JAVA:

```
import org.apache.commons.codec.binary.Base64;

import javax.crypto.Cipher;
import javax.crypto.spec.IvParameterSpec;
import javax.crypto.spec.SecretKeySpec;
import java.security.AlgorithmParameters;
import java.security.Key;

public class AESDemo {

    private static Key getKey(String key)  throws Exception{
        byte[] keyBytes = key.getBytes("UTF-8");
        SecretKeySpec newKey = new SecretKeySpec(keyBytes, "AES");
        return newKey;
    }
    private static AlgorithmParameters getIV(String iv) throws Exception {
        byte[] ivs = iv.getBytes("UTF-8");
        AlgorithmParameters params = AlgorithmParameters.getInstance("AES");
        params.init(new IvParameterSpec(ivs));
        return params;
    }

    public static String encrypt(String key,String iv,String text) throws Exception {

        Cipher cipher = Cipher.getInstance("AES/CBC/PKCS5Padding");
        cipher.init(Cipher.ENCRYPT_MODE, getKey(key), getIV(iv));
        byte[] encryptedBytes = cipher.doFinal(text.getBytes());
        return new String(Base64.encodeBase64(encryptedBytes, false,true),"UTF-8");
    }

    public static String decrypt(String key,String iv,String text) throws Exception {
        byte[] textBytes = Base64.decodeBase64(text);
        Cipher cipher = Cipher.getInstance("AES/CBC/PKCS5Padding");
        cipher.init(Cipher.DECRYPT_MODE, getKey(key), getIV(iv));
        byte[] decodedBytes = cipher.doFinal(textBytes);
        return new String(decodedBytes, "UTF-8");
    }

    public static void main(String[] args) throws Exception{
        String key="jHZZwiizsAF2qTAY";  //use your own Key here
        String iv="zijknVpNWeeTYGPV";  //use your own IV here
        String test="Hello World!";
        System.out.println(encrypt(key,iv,test));;
        System.out.println(decrypt(key,iv,encrypt(key,iv,test)));;
    }
}
```

## Data Forwarding

Lovense will notify you when the Model Status has changed, and you should send the Model Status back to the tipper.js. Here is the Flow Chart:

<img src="https://developer.lovense.com/assets/img/display-panel-intergrate.d495b55d.png" />

**How does it work?**

[Contact us](mailto:developer@mail.lovense.com) to provide your callback URL. Lovense will post `ModelStatus` to your callback URL when necessary (for example, when the model enables/disables features during their broadcast). You forward the ModelStatus to the tipper/viewer's page and reply to the callback request with "OK". Then call `Lovense.recieveData(data)` on the tipper/viewer's page.

**Description of the Callback Request**

**URL**: your callback url

**Request Protocol**: HTTPS Request

**Method**: POST

**Content Type**: application/json

**the ModelStatus Object**:
```
{
  "from":"ABCDxxxxxxxxxxxx", //Encrypted String using your AES KEY and IV
  "to": {
    "type":"customersOfModel", //which group of people you should forward to
    "target":"Lucy,Tony" // users you should forward the data to
  },
  "data": data // the data you should forward
}
```

**Attributes in ModelStatus**

- `from` [String]: Encrypted string of "Lovense" using your AES KEY. If you can decrypt it with your AES KEY, you can confirm that the data is from Lovense. It is a static string until you changed your AES KEY in Lovense Dashboard.
- `to` [Object]: Whom you should forward the data to
- `to.type` [String]: Which group of people you should forward the data to. Possible values: customersOfModel, customers
- `to.target` [String]: Users you should forward to
	- If the `to.type` is `customersOfModel`, the `to.target` is the model's name (separated with commas if there are multiple values), and you should forward the data to all the customers in the specified model's broadcasting room.
	- If the `to.type` is `customer`, the `to.target` is the tipper/viewer's screen name (separated with commas if there are multiple values), and you should forward the data to the specified tipper/viewer
- `data` [String]: the data that you should forward

**Here is a sample written in JAVA for handling the callback from Lovense:**
```
private final String KEY = "Your KEY";
private final String IV = "Your IV";
private final String AESFrom = AESUtil.encrypt("Lovense",KEY,IV);

@RequestMapping(value = "/lovense/callback")
public @ResponseBody String callback(@RequestBody Map<String,Object> modelStatus) {

  //1. If from is same as AESFrom
  if(AESFrom.equals(modelStatus.get("from"))){
    Map<String,String> to = (Map<String, String>) modelStatus.get("to");
    String data = (String) modelStatus.get("data");

    //2. Handle different target groups
    switch (to.get("type")){
      case "customersOfModel":
      String modelName = to.get("target");
      //3.1 TODO forward `data` to the tippers/viewers in the model's room
      break;
    case "customer":
      String customerScreenName = to.get("target");
      //TODO forward `data` to the tippers/viewers
      break;
    default:
      break;
    }
  }
  //4. Reply "OK"
  return "OK";
}
```

# Custom Cam Solution

## Basic API

This API allows developers to access Lovense toys through simple HTTPS request. Configure your settings on our developer dashboard before getting started.

### Configure developer dashboard

Go to the [developer dashboard](https://www.lovense.com/user/developer/info), select "Standard API" tab, enter your application info.

- **Website Name**: The name that will be displayed in Lovense Connect. Contact us if you want to change it.

- **Callback URL**: This is the URL from your server where our app sends callback information.

- **Heartbeat**: The callback interval for our app to update your server with the latest status

### Connect to Lovense toy(s)

We offer 2 ways to connect to our toys:

- Connect by Lovense Connect (iOS & Android version)

Users need to install Lovense Connect mobile app on their iOS/Android devices.

- [Download for Android](https://play.google.com/store/apps/details?id=com.lovense.connect)<br>
- [Download for iOS](https://itunes.apple.com/us/app/lovense-connect/id1273067916)
	
1. The user pairs their Lovense toy to the Lovense Connect app.

2. Get the QR code to connect to Lovense Connect.

Call Lovense Server API from your server

**API URL**: `https://api.lovense.com/api/lan/getQrCode`

> Notice: For security reasons, the developer token should be always used on the server side. You should never use it in your JS code from the client side.

**Request Protocol**: HTTPS Request

**Method**: POST

**Content Type**: application/json

**Parameters:**

Name	|Description	|Required
:-------|:--------------|:------
token	|Developer Token	|yes
uid	|User ID on your application	|yes
uname	|User ID on your application	|no
utoken	|User token from your website. This is for your own verification purposes. We suggest you generate a unique token for each user. This allows you to verify the user and avoid others faking the calls.	|no
v	|version	|yes


**Request Example:**
```
{
  "token": "FE1TxWpTciAl4E2QfYEfPLvo2jf8V6WJWkLJtzLqv/nB2AMos9XuWzgQNrbXSi6n",
  "uid": "42425232242",
  "v": 2
}
```

**Return:**
```
{
  code: 0
  result: true
  message: "Success"
  data: {
      "qr": "https://test2.lovense.com/UploadFiles/qr/20220106/xxxx.jpg", // qrcode picture
      "code": "xxxx"
  }
}
```

API will return a JSON object that contains the QR code image URL. You will get:

Name	|Description
:-------|:---
code	|Error code
message	|result
result	|true if successful, false if failed
data	|qrcode picture, code

Display the QR code image to the user

3. Users scan the QR code using Lovense Connect.

Once the user scans the QR code with the Lovense Connect app, the app will invoke the callback URL you provided on the developer dashboard.

Here's the JSON data Lovense Connect app will post to your server:
```
{
  uid: User ID
  utoken: xxx, // User token on your application
  domain: 192-168-0-11.lovense.club, // the domain name of the Lovense Connect app
  httpPort: 34567, // HTTP server port
  wsPort: 34567, // HTTP server port
  httpsPort: 34568, //HTTPS server port
  wssPort: 34568, // HTTPS server port
  platform: 'ios', // the Lovense Connect platform e.g. Android/iOS
  appVersion: '2.3.6', //the Lovense Connect version
  version: 101, // the protocol version e.g. 101
  toys:{
      toyId:{
        id: xxxxxx, // Toy's ID
        name:"lush", // toy's name
        nickName: "nickName"
        status: 1 // e.g. 1/0
      }
    }
}
```
The `domain` and `httpPort` used to connect to Lovense Connect (iOS & Android) are returned.

- Connect by Lovense Connect (PC & Mac version) on the same computer

- [Download for Windows](https://www.lovense.com/files/apps/connect/Lovense_Connect.exe)
- [Download for Mac](https://www.lovense.com/files/apps/connect/Lovense_Connect.dmg)

Windows computers - Users need the Lovense USB Bluetooth Adapter.

Mac - Users can connect our toys directly to their Mac via Bluetooth.

The user pairs their Lovense toy to Lovense Connect for PC/Mac.


### Command the toy(s)

Once everything is set up, you can command our toys:

- **Local APIs**: These APIs are exposed by Lovense Connect app through HTTPS. The connections are in the same LAN or the same computer.
- **Server APIs**: If your application can’t establish a LAN connection to the user’s Lovense Connect, you can use the Server API to connect the user’s toy.

#### **By local application**
> ⚠️ iOS Connect v2.6.4, Android Connect v2.7.7, PC Connect 1.5.4 are required.

If the user's device is in the same LAN environment, a POST request to Lovense Connect can be used to trigger a toy response. In this case, both your server and Lovense's server are not required.

If the user has Lovense Connect mobile, the `domain` and `httpsPort` are accessed from the callback information. If the user has Lovense Connect for PC/Mac, the `domain` is `127-0-0-1.lovense.club`, and the `httpsPort` is `30010`.

With the same command line, different parameters will lead to different results as below.

- GetToys Request

Get the user's toy(s) information.

**API URL**: https://{domain}:{httpsPort}/command

**Request Protocol**: HTTPS Request

**Method**: POST

**Request Content Type**: application/json

**Response Format**: JSON

**Parameters:**

Name	|Description	|Type	|Note	|Required
:---|:---|:---|:---|:---|
command	|Type of request	|String	|/	|yes

**Request Example:**
```
{
  "command": "GetToys"
}
```

**Response Example:**
```
{
  "code": 200,
  "data": {
    "toys": "{  \"fc9f37e96593\" : {    \"id\" : \"fc9f37e96593\",    \"status\" : \"1\",    \"version\" : \"\",    \"name\" : \"nora\",    \"battery\" : 100,    \"nickName\" : \"\"  }}",
    "platform": "ios",
    "appType": "connect"
  },
  "type": "OK"
}
```

- Function Request

**API URL**: https://{domain}:{httpsPort}/command

**Request Protocol**: HTTPS Request

**Method**: POST

**Request Content Type**: application/json

**Response Format**: JSON

**Parameters:**

Name	|Description	|Type	|Note	|Required
:---|:---|:---|:---|:---|
command	|Type of request	|String	|/	|yes
action	|Control the function and strength of the toy	|string	|Actions can be Vibrate, Rotate, Pump or Stop. Use Stop to stop the toy’s response.Range:Vibrate:0 ~ 20Rotate: 0~20Pump:0~3	|yes
timeSec	|Total running time	|double	0 = indefinite length <br>Otherwise, running time should be greater than `1`.	|yes
loopRunningSec	|Running time	|double	|Should be greater than `1`.	|no
loopPauseSec	|Suspend time	|double	|Should be greater than `1`.	|no
toy	|Toy ID	|string	|Optional, if you don’t include this, it will be applied to all toys	|no
stopPrevious	|Stop all previous commands and execute current commands	|int	|Optional, Default: `1`, If set to `0` , it will not stop the previous command.	|no
apiVer	|The version of the request	|int	|Always use `1`	|yes

> Parameter `stopPrevious` are available in the following versions: Android Connect 2.7.6, iOS Connect 2.7.2, PC Connect 1.5.9 .

**Request Example:**
```
// Vibrate toy ff922f7fd345 at 16th strength, run 9 seconds then suspend 4 seconds. It will be looped. Total running time is 20 seconds.
{
  "command": "Function",
  "action": "Vibrate:16",
  "timeSec": 20,
  "loopRunningSec": 9,
  "loopPauseSec": 4,
  "toy": "ff922f7fd345",
  "apiVer": 1
}
```
```
// Vibrate 9 seconds at 2nd strength
// Rotate toys 9 seconds at 3rd strength
// Pump all toys 9 seconds at 4th strength
// For all toys, it will run 9 seconds then suspend 4 seconds. It will be looped. Total running time is 20 seconds.
{
  "command": "Function",
  "action": "Vibrate:2,Rotate:3,Pump:4",
  "timeSec": 20,
  "loopRunningSec": 9,
  "loopPauseSec": 4,
  "apiVer": 1
}
```
- Pattern Request

If you want to change the way the toy responds very frequently you can use a pattern request. To avoid network pressure and obtain a stable response, use the commands below to send your predefined patterns at once.

**API URL**: https://{domain}:{httpsPort}/command

**Request Protocol**: HTTPS Request

**Method**: POST

**Request Content Type**: application/json

**Response Format**: JSON

**Parameters:**

Name	|Description	|Type	|Note	|Required
:---|:---|:---|:---|:---|
command	|Type of request	|String	|/	|yes
rule	|"V:1;F:vrp;S:1000#" <br>V:1; Protocol version, this is static; <br>F:vrp; Features: v is vibrate, r is rotate, p is pump, this should match the strength below; <br>S:1000; Intervals in Milliseconds, should be greater than 100.	|string	|The strength of r and p will automatically correspond to v.	|yes
strength	|The pattern <br>For example: 20;20;5;20;10	|string	|No more than 50 parameters. Use semicolon `;` to separate each strength.	|yes
timeSec	|Total running time	|double|	0 = indefinite length <br>Otherwise, running time should be greater than `1`.	|yes
toy	|Toy ID	|string	|Optional, if you don’t include this, it will apply to all toys.	|no
apiVer	|The version of the request	|int	|Always use `1`	|yes

**Request Example:**
```
// Vibrate the toy as defined. The interval between changes is 1 second. Total running time is 9 seconds.
{
  "command": "Pattern",
  "rule": "V:1;F:v;S:1000#",
  "strength": "20;20;5;20;10",
  "timeSec": 9,
  "toy": "ff922f7fd345",
  "apiVer": 1
}
```
```
// Vibrate the toys as defined. The interval between changes is 0.1 second. Total running time is 9 seconds.
// If the toys include Nora or Max, they will automatically rotate or pump, you don't need to define it.
{
  "command": "Pattern",
  "rule": "V:1;F:vrp;S:100#",
  "strength": "20;20;5;20;10",
  "timeSec": 9,
  "apiVer": 1
}
```
- Preset Request

**API URL**: https://{domain}:{httpsPort}/command

**Request Protocol**: HTTPS Request

**Method**: POST

**Request Content Type**: application/json

**Response Format**: JSON

**Parameters:**

Name	|Description	|Type	|Note	|Required
:---|:---|:---|:---|:---|
command	|Type of request	|String	|/	|yes
name	|Preset pattern name	|string	|We provide four preset patterns in the Lovense Connect app: pulse, wave, fireworks, earthquake	|yes
timeSec	|Total running time	|double	0 = indefinite length <br>Otherwise, running time should be greater than `1`.	|yes
toy	|Toy ID	|string	|Optional, if you don’t include this, it will be applied to all toys.	|no
apiVer	|The version of the request	|int	|Always use `1`	|yes

**Request Example:**
```
// Vibrate the toy with pulse pattern, the running time is 9 seconds.
{
  "command": "Preset",
  "name": "pulse",
  "timeSec": 9,
  "toy": "ff922f7fd345",
  "apiVer": 1
}
```
**Response Example:**
```
{
  "code": 200,
  "type": "ok"
}
```

**Error Codes:**

Code	|Message
:---|:---
500	|HTTP server not started or disabled
400	|Invalid Command
401	|Toy Not Found
402	|Toy Not Connected
403	|Toy Doesn't Support This Command
404	|Invalid Parameter
506	|Server Error. Restart Lovense Connect


#### **By server application**
If your application can’t establish a LAN connection to the user’s Lovense Connect, you can use the Server API to connect the user’s toy.

> ⚠️ Coming soon to PC Connect.

- Function Request

**API URL:** https://api.lovense.com/api/lan/v2/command

**Request Protocol:** HTTPS Request

**Method:** POST

**Request Content Type:** application/json

**Request Format:** JSON

**Parameters:**

Name	|Description	|Type	|Note	|Required
:---|:---|:---|:---|:---|
token	|Your developer token	|string		|yes
uid	|Your user’s ID	|string	|To send commands to multiple users at the same time, add all the user IDs separated by commas. The toy parameter below will be ignored and the commands will go to all user toys by default.	|yes
command	|Type of request	|String	|/	|yes
action	|Control the function and strength of the toy	|string	|Actions can be Vibrate, Rotate, Pump or Stop. Use Stop to stop the toy’s response.Range:Vibrate:0 ~ 20Rotate: 0~20Pump:0~3	|yes
timeSec	|Total running time	|double	|0 = indefinite length <br>Otherwise, running time should be greater than `1`.	|yes
loopRunningSec	|Running time	|double	|Should be greater than `1`	|no
loopPauseSec	|Suspend time	|double	|Should be greater than `1`	|no
toy	|Toy ID	|string	|Optional, if you don’t include this, it will be applied to all toys.	|no
stopPrevious	|Stop all previous commands and execute current commands	|int	|Optional, Default: `1`, If set to `0` , it will not stop the previous command.	|no
apiVer	|The version of the request	|int	|Always use `1`	|yes

> Parameter `stopPrevious` are available in the following versions: Android Connect 2.7.6, iOS Connect 2.7.2 PC Connect 1.5.9 .

**Request Example:**
```
// Vibrate toy ff922f7fd345 at 16th strength, run 9 seconds then suspend 4 seconds. It will be looped. Total running time is 20 seconds.
{
  "token": "FE1TxWpTciAl4E2QfYEfPLvo2jf8V6WJWkLJtzLqv/nB2AMos9XuWzgQNrbXSi6n",
  "uid": "1132fsdfsd",
  "command": "Function",
  "action": "Vibrate:16",
  "timeSec": 20,
  "loopRunningSec": 9,
  "loopPauseSec": 4,
  "apiVer": 1
}
```
```
// Vibrate 9 seconds at 2nd strength
// Rotate toys 9 seconds at 3rd strength
// Pump all toys 9 seconds at 4th strength
// For all toys, it will run 9 seconds then suspend 4 seconds. It will be looped. Total running time is 20 seconds.
{
  "token": "FE1TxWpTciAl4E2QfYEfPLvo2jf8V6WJWkLJtzLqv/nB2AMos9XuWzgQNrbXSi6n",
  "uid": "1132fsdfsd",
  "command": "Function",
  "action": "Vibrate:2,Rotate:3,Pump:4",
  "timeSec": 20,
  "loopRunningSec": 9,
  "loopPauseSec": 4,
  "apiVer": 1
}
```
```
// Stop all toys
{
  "command": "Function",
  "action": "Stop",
  "timeSec": 0,
  "apiVer": 1
}
```

- Pattern Request

If you want to change the way the toy responds very frequently you can use a pattern request. To avoid network pressure and obtain a stable response, use the commands below to send your predefined patterns at once.

**API URL:** https://api.lovense.com/api/lan/v2/command

**Request Protocol:** HTTPS Request

**Method:** POST

**Request Content Type:** application/json

**Request Format:** JSON

**Parameters:**

Name	|Description	|Type	|Note	|Required
:---|:---|:---|:---|:---|
token	|Your developer token	|string		|yes
uid	|Your user’s ID|	string	|To send commands to multiple users at the same time, add all the user IDs separated by commas. The toy parameter below will be ignored and the commands will go to all user toys by default.	|yes
command	|Type of request	|String	|/	|yes
rule	|"V:1;F:vrp;S:1000#" <br>V:1; Protocol version, this is static; <br>F:vrp; Features: v is vibrate, r is rotate, p is pump, this should match the strength below; <br>S:1000; Intervals in Milliseconds, should be greater than 100.	|string	|The strength of r and p will automatically correspond to v.	|yes
strength	|The pattern <br>For example: 20;20;5;20;10	|string	|No more than 50 parameters. Use semicolon `;` to separate each strength.	|yes
timeSec	|Total running time	|double|	0 = indefinite length <br>Otherwise, running time should be greater than `1`.	|yes
toy	|Toy ID	|string	|Optional, if you don’t include this, it will apply to all toys.	|no
apiVer	|The version of the request	|int	|Always use `1`	|yes

**Request Example:**
```
// Vibrate the toy as defined. The interval between changes is 1 second. Total running time is 9 seconds.
{
  "token": "FE1TxWpTciAl4E2QfYEfPLvo2jf8V6WJWkLJtzLqv/nB2AMos9XuWzgQNrbXSi6n",
  "uid": "1ads22adsf",
  "command": "Pattern",
  "rule": "V:1;F:v;S:1000#",
  "strength": "20;20;5;20;10",
  "timeSec": 9,
  "apiVer": 1
}
```
```
// Vibrate the toys as defined. The interval between changes is 0.1 second. Total running time is 9 seconds.
// If the toys include Nora or Max, they will automatically rotate or pump, you don't need to define it.
{
  "token": "FE1TxWpTciAl4E2QfYEfPLvo2jf8V6WJWkLJtzLqv/nB2AMos9XuWzgQNrbXSi6n",
  "uid": "1ads22adsf",
  "command": "Pattern",
  "rule": "V:1;F:vrp;S:100#",
  "strength": "20;20;5;20;10",
  "timeSec": 9,
  "apiVer": 1
}
```

- Preset Request

**API URL:** https://api.lovense.com/api/lan/v2/command

**Request Protocol:** HTTPS Request

**Method:** POST

**Request Content Type:** application/json

**Request Format:** JSON

**Parameters:**

Name	|Description	|Type	|Note	|Required
:---|:---|:---|:---|:---|
token	|Your developer token	|string		|yes
uid	|Your user’s ID|	string	|To send commands to multiple users at the same time, add all the user IDs separated by commas. The toy parameter below will be ignored and the commands will go to all user toys by default.	|yes
command	|Type of request	|String	|/	|yes
name	|Preset pattern name	|string	|We provide four preset patterns in the Lovense Remote app: pulse, wave, fireworks, earthquake	|yes
timeSec	|Total running time	|double|	0 = indefinite length <br>Otherwise, running time should be greater than `1`.	|yes
toy	|Toy ID	|string	|Optional, if you don’t include this, it will apply to all toys.	|no
apiVer	|The version of the request	|int	|Always use `1`	|yes


**Request Example:**
```
// Vibrate the toy with pulse pattern, the running time is 9 seconds.
{
  "token": "FE1TxWpTciAl4E2QfYEfPLvo2jf8V6WJWkLJtzLqv/nB2AMos9XuWzgQNrbXSi6n",
  "uid": "1adsf2323",
  "command": "Preset",
  "name": "pulse",
  "timeSec": 9,
  "apiVer": 1
}
```

**Response Example:**
```
{
  "result": true,
  "code": 200,
  "message": "Success"
}
```

**Server Error Codes:**

Code	|Message
:---|:---
200	|Success
400	|Invalid command
404	|Invalid Parameter
501	|Invalid token
502	|You do not have permission to use this API
503	|Invalid User ID
507	|Lovense APP is offline



## Basic JS API

This solution allows you to control Lovense toys through your webpage.

Your users must pair their toy(s) to the Lovense Connect app for mobile or PC.

> ⚠️ iOS Connect v2.6.4+, Android Connect v2.7.7+, or PC Connect 1.5.4+ is required.

> ⚠️ User's devices must be in the same LAN.

### Set your callback URL

Go to the [developer dashboard](https://www.lovense.com/user/developer/info) and set your Callback URL.

### Import the LAN.JS to your user's page

Add our LAN.JS to your user's page.

```
<script src="https://api.lovense.com/api/lan/v2/lan.js"></script>
```

### Handle the callback data from Lovense Connect

You can listen to the callback from Lovense Connect as below:
```
// the path should be the same as you set in the Lovense developer dashboard
@RequestMapping(value = "/api/lovense/callback")
public @ResponseBody
MessageResponse callback(@RequestBody Map body) {
  //TODO
  //send the callback data to the user's page and call `lovense.setConnectCallbackData` there
  return new MessageResponse(true, "success");
}
```
Call `lovense.setConnectCallbackData` with the callback data you received from Lovense Connect in the user's page to initialize the LAN.JS.
```
lovense.setConnectCallbackData(callbackData)
```
> ⚠️ If the user uses Lovense Connect for PC, this step can be skipped because a QR code is not required.

### Command the toy(s)

Send commands to Lovense toy(s) by calling `lovense.sendCommand(parameters)`.

For the parameters list, see [Command the toy(s)].

Example:
```
lovense.sendCommand({
  command: "Function",
  action: "Vibrate:16",
  timeSec: 20,
  loopRunningSec: 9,
  loopPauseSec: 4,
  toy: "ff922f7fd345",
  apiVer: 1,
})
```

### Other methods

- getToys

Get all toys, it will return all toys as an array

```
lovense.getToys()
```
- getOnlineToys

Get all online toys, it will only return the connected toys as an array

```
lovense.getOnlineToys()
```

- isToyOnline

Determine whether the toy is connected, it will return true as long as there is a toy connected.
```
lovense.isToyOnline()
```
- openMobileConnect

Open Lovense Mobile Connect.
```
lovense.openMobileConnect()
```

GIVE FEEDBACK
-------------
Please report bugs or issues to [Lovense Support](mailto:developer@mail.lovense.com).

You can also join our [Lovense Developer Community Discord](https://discord.gg/dW9f54BwqR), visit the [Lovense Developers Website](https://developer.lovense.com/#introduction), or open an issue in this repository.
