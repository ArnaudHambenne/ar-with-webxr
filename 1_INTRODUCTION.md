# 1. Introduction
This codelab will go through an example of building an AR web application. It uses JavaScript to render 3D models that appear as if they exist in the real world.

You will use the still-in-development [WebXR Device API](https://immersive-web.github.io/webxr/), (the successor to the [WebVR API](https://developer.mozilla.org/en-US/docs/Web/API/WebVR_API)), that combines both augmented reality (AR) and virtual reality (VR) functionality. We'll be focusing on experimental AR extensions to the WebXR Device API that are being developed in Chrome.

<img src="https://codelabs.developers.google.com/codelabs/ar-with-webxr/img/3f852a07a7a280d6.png" width="80%">

#### What is Augmented Reality?
Augmented reality (AR) is a term usually used to describe the mixing of computer-generated graphics with the real world, which, in the case of phone-based Augmented Reality, means convincingly placing computer graphics over a live camera feed. In order for this effect to remain convincing as the phone moves through the world, the AR-enabled device needs to understand the world it is moving through, which may include detecting surfaces and estimating lighting of the environment. Additionally, the device also needs to determine its "pose" (position and orientation) in 3D space.

Augmented reality usage has been increasing, with the popularity of AR usage in apps like selfie filters and AR-based games. Today, there are already hundreds of millions of AR-enabled smartphones, less than a year after the release of [ARCore](https://developers.google.com/ar/discover/), Google's augmented reality platform, and [ARKit](https://developer.apple.com/arkit/) by Apple. With the technology now in the hands of millions of people, initial proposals of AR extensions for the WebXR Device API can be experimentally implemented behind flags.

#### What you will build
<img align="right" width="213" height="379" src="https://codelabs.developers.google.com/codelabs/ar-with-webxr/img/266f0ac0b7f505fc.png">
In this codelab, you're going to build a web application that places a model in the real world using augmented reality. Your app will:

1. Use your device's sensors to determine and track its position and orientation in the world.
2. Render a 3D model composited on top of a live camera view.
3. Execute hit tests to place objects on top of discovered surfaces in the real world.
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

---
#### What you'll learn
* How to use the WebXR Device API
* How to find a surface using augmented reality hit tests
* How to load and render a 3D model synchronized with the real world camera feed

This codelab is focused on augmented reality APIs. Non-relevant concepts and code blocks are glossed over and are provided for you in the corresponding repository code.