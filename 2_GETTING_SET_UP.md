# 2. Getting set up
##### :heavy_exclamation_mark: **Will only work in Chrome 74 or higher** :heavy_exclamation_mark:

The WebXR Device API is undergoing a lot of changes currently. This codelab will only work in Chrome versions 74 and up.

### What you'll need
This is an overview of what you'll need, and we'll go into more detail shortly.

* A workstation for coding and hosting static web content
* [ARCore-capable Android device](https://developers.google.com/ar/discover/#supported_devices) running at least [Android 8.0 Oreo](https://www.android.com/versions/oreo-8-0/)
* [ARCore](https://play.google.com/store/apps/details?id=com.google.ar.core) installed (Chrome will automatically prompt you to install ARCore)
* [Chrome 74 or higher](https://www.google.com/chrome/beta). You'll need a version of Chrome that's 74 or higher, and use a Beta, Dev or Canary build of Chrome
* [Web Server for Chrome](https://chrome.google.com/webstore/detail/web-server-for-chrome/ofhbbkphhbklhfoeikjpcbhemlocgigb), or your own web server of choice
* USB cable to connect your AR device to workstation
* [The sample code](https://github.com/arnaudhambenne/ar-with-webxr/archive/master.zip)
* A text editor
* Basic knowledge of HTML, CSS, JavaScript, and [Chrome DevTools](https://developer.chrome.com/devtools)

### Ensure AR features are enabled on Chrome
At the time of writing, the initial AR features are behind both `webxr` and `webxr-hit-test` flags. To enable WebXR augmented reality support in Chrome:

1. Verify that you're running at least [Android 8.0 Oreo](https://www.android.com/versions/oreo-8-0/)
2. Verify that your Android phone is one of the [supported](https://developers.google.com/ar/discover/#supported_devices) ARCore devices
3. Confirm Chrome version is >= 74
4. Type `chrome://flags` in the URL bar
5. Type `webxr` in the Search flags input field
6. Set the **WebXR Device API** (`#webxr`) flag to **Enabled**
* Note, ignore the similar looking **WebVR** (`#enable-webvr`) flag
7. Set the **WebXR Hit Test** (`#webxr-hit-test`) flag to **Enabled**
8. Tap RELAUNCH NOW to ensure the updated flags take effect

<img src="https://codelabs.developers.google.com/codelabs/ar-with-webxr/img/d1b234e7d6e04a2a.png" width="30%">

Visit the link below **on your AR device** to try the first step of this Codelab. If you get a page with a message displaying "Your browser does not have AR features", re-check the version of Chrome and the WebXR flags, which might require a browser restart.

[TRY IT](https://arnaudhambenne.github.io/ar-with-webxr/work/)

### Download the Code
Click the following link to download all the code for this codelab **on your workstation**:

[Download source code](https://github.com/arnaudhambenne/ar-with-webxr/archive/master.zip)

Unpack the downloaded zip file. This will unpack a root folder (`ar-with-webxr-master`), which contains directories of several steps of this codelab, along with all the resources you need. The `step-05` and `step-06` folders contain the desired end state of the 5th and 6th steps of this codelab, as well as the `final` result. They are there for reference. We'll be doing all our coding work in a directory called `work`.

### Install and verify web server
You're free to use your own web server, but we'll walk you through using the Chrome Web Server if you don't have one set up. If you don't have that app installed **on your workstation** yet, you can install it from the Chrome Web Store.

[Install Web Server for Chrome](https://chrome.google.com/webstore/detail/web-server-for-chrome/ofhbbkphhbklhfoeikjpcbhemlocgigb?hl=en)

After installing the Web Server for Chrome app, go to chrome://apps and click on the Web Server icon:

<img src="https://codelabs.developers.google.com/codelabs/ar-with-webxr/img/dc07bbc9fcfe7c5b.png" width="10%">

You'll see this dialog next, which allows you to configure your local web server:

<img src="https://codelabs.developers.google.com/codelabs/ar-with-webxr/img/b91087c4a372ee8d.png">

1. Click the **choose folder** button, and select the `ar-with-webxr-master` folder. This will enable you to serve your work in progress via the URL highlighted in the web server dialog (in the **Web Server URL(s)** section).
2. Under Options, make sure **Automatically show index.html** is checked.
3. **STOP** and **RESTART** the server by sliding the toggle labeled Web Server: **STARTED/STOPPED** to the left, and then back to the right.
<img src="https://codelabs.developers.google.com/codelabs/ar-with-webxr/img/daefd30e8a290df5.png" width="30%">
4. **Verify that at least one Web Server URL(s) appears:**
* **http://127.0.0.1:8887** — the default localhost URL

Now we also want to configure our AR device such that visiting `localhost:8887` on our AR device will access the same port on our workstation.
1. On you development workstation, go to `chrome://inspect` and click the **Port forwarding...** button:
![](https://codelabs.developers.google.com/codelabs/ar-with-webxr/img/9198a15678e90e07.png)

Use the **Port forwarding settings** dialog to forward port `8887` to `localhost:8887`. Ensure that **Enable port forwarding** is checked:

<img src="https://codelabs.developers.google.com/codelabs/ar-with-webxr/img/8ceaaff488b3161.png" width="40%">

Test your connection:

1. Connect your AR device to your workstation via a USB cable.
2. On your AR device in Chrome Canary enter `http://localhost:8887` into the URL bar.
Your AR device should forward this request to your development workstation's web server. You should see a directory of files.
3. On your AR device, tap on the `work` directory to load the `work/index.html` page.




You should see page that contains an **ENTER AUGMENTED REALITY** button...  |  However, if you see an **Unsupported Browser** error page, go back and confirm the Chrome Canary version, chrome://flags and restart Chrome.
:-------------------------:|:-------------------------:
![](https://codelabs.developers.google.com/codelabs/ar-with-webxr/img/2960fdfd01572a73.png)  |  <img src="https://codelabs.developers.google.com/codelabs/ar-with-webxr/img/eb923e4c74e0a8a5.png" width="82%">

1. Once the connection to your web server is working with your AR device, click the **ENTER AUGMENTED REALITY** button.
You may be prompted to install ARCore:
<img src="https://codelabs.developers.google.com/codelabs/ar-with-webxr/img/d9fa833e7c75fbf8.png" width="40%">


2. The first time you run an AR application you'll see a camera permissions prompt:



| <img src="https://codelabs.developers.google.com/codelabs/ar-with-webxr/img/32d7ef08a7216eb8.png" width="60%">  |  <img src="https://codelabs.developers.google.com/codelabs/ar-with-webxr/img/92b0afd1dc7915e.png" width="46%">
:-------------------------:|:-------------------------:


Once everything is good to go, there should be a scene of cubes overlayed on top of a camera feed. The scene understanding improves as more of the world is parsed by the camera, so some moving around can help stabilize things.

<img src="https://codelabs.developers.google.com/codelabs/ar-with-webxr/img/53edad20e6426c9c.png" width="85%">


> **Important**: For security reasons, the WebXR Device API is only able to run in secure (HTTPS) environments, with an exception made for `localhost` development. If you have issues activating WebXR, ensure you're using a secure document, or a `localhost` URL.

> From this point forward, all testing/verification (e.g. the **Test It Out** sections in subsequent steps) require visiting the link on your AR device.

<br>

<div align="right"><a href="#" align="left">:point_right: 3. State of AR on the web</a></div>
