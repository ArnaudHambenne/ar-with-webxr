# 3. State of AR on the web
### History
[WebGL](https://developer.mozilla.org/docs/Web/API/WebGL_API) is a powerful graphics library enabling the rendering of 
3D content on the web, but access to VR devices from the web are necessary for discovery, refresh rate synchronization 
and positioning. The experimental [WebVR 1.1 API](https://developer.mozilla.org/en-US/docs/Web/API/WebVR_API) has been 
implemented in browsers as web developers explored building VR applications for the web. This provided the framework to 
render a stereoscopic web scene with appropriate distortions for a VR headset. [Daydream](https://vr.google.com/daydream/)- 
and [GearVR](http://www.samsung.com/global/galaxy/gear-vr/)- enabled mobile devices and full room systems like the 
[Oculus Rift](https://www.oculus.com/rift/) or [HTC Vive](https://www.vive.com/) are some of the platforms 
[supported](https://webvr.info/) by different browsers.

As the industry and use cases evolved, so did the need to support AR on the web. Due to the technological similarities 
between AR and VR, the [WebXR Device API](https://immersive-web.github.io/webxr/) was created to encompass both. While 
still in-development and subject to change, the core of the WebXR Device API is mostly stable, and is being developed 
by representatives from all major browser vendors. The API supports VR experiences, with initial AR proposals just now 
being prototyped and explored.

### Implementation
The first implementation of the WebXR Device API were made available in Chrome 67 behind a flag (`#webxr`) and as an 
[origin trial](https://github.com/GoogleChrome/OriginTrials). The initial experimental AR features are in Chrome 70+ 
behind a flag, `#webxr-hit-test`. At the time of writing, all browsers with Web**VR** implementations have committed to 
supporting the Web**XR** Device API in the future.

### The Future
The only scene understanding currently available to the browser is a "hit test" feature. This allows you to cast a ray 
out from the device, for example based on a user screen tap, and return any collisions with the real world, allowing us 
to use that information to overlay virtual scenes.

Future explorations may expand upon scene understanding, providing things like light estimation, surfaces, meshes, 
feature point clouds, and more.

<br>

<div align="right"><a href="#" align="left">:point_right: Overview of our AR app</a></div>

