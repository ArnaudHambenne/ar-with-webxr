# 2. Getting set up
#### :heavy_exclamation_mark: **Will only work in latest Chrome Dev/Canary (74/75)** :heavy_exclamation_mark:

The WebXR Device API is undergoing a lot of changes currently. This codelab will only work in Chrome Canary/Dev versions 74-75.

### What you'll need
This is an overview of what you'll need, and we'll go into more detail shortly.

* A workstation for coding and hosting static web content
* [ARCore-capable Android device](https://developers.google.com/ar/discover/#supported_devices) running at lease [Android 8.0 Oreo](https://www.android.com/versions/oreo-8-0/)
* ARCore installed (Chrome will automatically prompt you to install ARCore)
* [Chrome Dev/Canary](https://www.google.com/chrome/dev). You'll need a version of Chrome that's 74-75, and use a Canary or Dev build of Chrome
* [Web Server for Chrome](https://chrome.google.com/webstore/detail/web-server-for-chrome/ofhbbkphhbklhfoeikjpcbhemlocgigb), or your own web server of choice
* USB cable to connect your AR device to workstation
* The sample code - Download a [zip](https://github.com/arnaudhambenne/ar-with-webxr/archive/master) or clone this repo
* A text editor
* Basic knowledge of HTML, CSS, JavaScript, and Chrome DevTools
### Get Chrome with AR Features
At the time of writing, the initial AR features are only implemented in Chrome Canary builds with minimum versions of 70. You can go to Settings -> About Chrome to see what version of Chrome you're using.

### Ensure AR features are enabled on Chrome
At the time of writing, the initial AR features are behind both webxr and webxr-hit-test flags. To enable WebXR augmented reality support in Chrome:

Verify that you're running Android 8.0 Oreo
Verify that your Android phone is one of the supported ARCore devices
Confirm Chrome version is >= 70
Type chrome://flags in the URL bar
Type webxr in the Search flags input field
Set the WebXR Device API (#webxr) flag to Enabled
Note, ignore the similar looking WebVR (#enable-webvr) flag
Set the WebXR Hit Test (#webxr-hit-test) flag to Enabled
Tap RELAUNCH NOW to ensure the updated flags take effect


Visit the link below on your AR device to try the first step of this Codelab. If you get a page with a message displaying "Your browser does not have AR features", re-check the version of Chrome Canary and the WebXR flags, which might require a browser restart.


### Download the Code
Click the following link to download all the code for this codelab on your workstation:


Unpack the downloaded zip file. This will unpack a root folder (ar-with-webxr-master), which contains directories of several steps of this codelab, along with all the resources you need. The step-05 and step-06 folders contain the desired end state of the 5th and 6th steps of this codelab, as well as the final result. They are there for reference. We'll be doing all our coding work in a directory called work.

### Install and verify web server
You're free to use your own web server, but we'll walk you through using the Chrome Web Server if you don't have one set up. If you don't have that app installed on your workstation yet, you can install it from the Chrome Web Store.


After installing the Web Server for Chrome app, go to chrome://apps and click on the Web Server icon:



You'll see this dialog next, which allows you to configure your local web server:



Click the choose folder button, and select the ar-with-webxr-master folder. This will enable you to serve your work in progress via the URL highlighted in the web server dialog (in the Web Server URL(s) section).
Under Options, make sure Automatically show index.html is checked.
STOP and RESTART the server by sliding the toggle labeled Web Server: STARTED/STOPPED to the left, and then back to the right.

Verify that at least one Web Server URL(s) appears:
http://127.0.0.1:8887 — the default localhost URL
Now we also want to configure our AR device such that visiting localhost:8887 on our AR device will access the same port on our workstation.

On you development workstation, go to chrome://inspect and click the Port forwarding... button:

Use the Port forwarding settings dialog to forward port 8887 to localhost:8887. Ensure that Enable port forwarding is checked:


Test your connection:

Connect your AR device to your workstation via a USB cable.
On your AR device in Chrome Canary enter http://localhost:8887 into the URL bar.
Your AR device should forward this request to your development workstation's web server. You should see a directory of files.
On your AR device, tap on the work directory to load the work/index.html page.
You should see page that contains an ENTER AUGMENTED REALITY button...

...However, if you see an Unsupported Browser error page, go back and confirm the Chrome Canary version, chrome://flags and restart Chrome.





Once the connection to your web server is working with your AR device, click the ENTER AUGMENTED REALITY button.
You may be prompted to install ARCore:

The first time you run an AR application you'll see a camera permissions prompt:
 → 
Once everything is good to go, there should be a scene of cubes overlayed on top of a camera feed. The scene understanding improves as more of the world is parsed by the camera, so some moving around can help stabilize things.



Important: For security reasons, the WebXR Device API is only able to run in secure (HTTPS) environments, with an exception made for localhost development. If you have issues activating WebXR, ensure you're using a secure document, or a localhost URL.

From this point forward, all testing/verification (e.g. the Test It Out sections in subsequent steps) require visiting the link on your AR device.