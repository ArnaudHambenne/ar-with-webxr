# 4. Overview of our AR app
We've provided an HTML page with CSS styling and JavaScript for enabling basic AR functionality that renders a scene of cubes in sync with your camera's position. This speeds up the setup process and allows this codelab to focus on the AR features.

> We've given you the markup and styles to save you some time and make sure you're starting on a solid foundation. In the next section, you'll have an opportunity to write your own code. If you're familiar with the WebXR Device API or want to get right to the augmented reality features, continue to the next section.

### The HTML page
We're building an AR experience into a traditional webpage using existing web technologies. In this specific experience, we'll use a full screen rendering canvas, so our HTML file doesn't need to have too much complexity. The CSS ensures the `<canvas>` injected by our graphics library is fullscreen. The HTML page loads the scripts.

AR features require a user gesture to initiate, so there are some [Material Design Lite](https://getmdl.io/) elements for displaying the "Start AR" button, and the unsupported browser message.

The `index.html` file that is already in your `work` directory should look something like this (this is a subset of the actual contents, **don't copy this code into your file**):

```html
<!doctype html>
<html>
  <head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Building an augmented reality application with the WebXR Device API</title>
    <link rel="stylesheet" type="text/css" href="../shared/app.css" />
    <link rel="stylesheet" type="text/css" href="../third_party/mdl/material.min.css" />
  </head>
  <body>
    <div id="enter-ar-info" class="demo-card mdl-card mdl-shadow--4dp">
      <!-- Material Design elements for demo --> 
      <!-- ... -->
    </div>
    <script src="../third_party/three.js/three.js"></script>
    <script src="../shared/utils.js"></script>
    <script src="app.js"></script>
  </body>
</html>
```

### Check out the key JavaScript code
Our app consists of using the 3D JavaScript library [three.js](http://threejs.org/), some utilities and all of our WebXR/app-specific code in `app.js`. Let's walk through our app's boilerplate.

> This boilerplate uses async functions. If you're unfamiliar with async functions, check out this great Web Fundamentals article, [Async functions - making promises friendly](https://developers.google.com/web/fundamentals/primers/async-functions).

Your work directory also already includes the app code (`app.js`), in it you'll find the `App` class:

```javascript
class App {
  constructor() {
    ...
  }

  async init() {
    ...
  }

  async onEnterAR() {
    ...
  }

  onNoXR() {
    ...
  }

  async onSessionStarted(session) {
   ...
  }

  onXRFrame(time, frame) {
    ...
  }
};

window.app = new App();
```

We instantiate our app and store it as `window.app` for convenience while using [Chrome DevTools](https://developer.chrome.com/devtools) to debug.

Our constructor calls `this.init()` which is an async function that will start up our [XRSession](https://immersive-web.github.io/webxr/#xrsession-interface) for working with AR. This function checks for the existence of `navigator.xr`, the entry point for the WebXR Device API, as well as `XRSession.prototype.requestHitTest`, the AR feature enabled by the `webxr-hit-test` Chrome flag.

* If `navigator.xr` doesn't exist or `XRSession.prototype.requestHitTest` doesn't exist, then we call `this.onNoXR()` which displays a message indicating lack of AR support.
  * *note: the previous version of this tutorial used `navigator.xr.requestDevice()`, which has since been moved to the `XRSession` object.*

If all is well, we bind a click listener on our "Enter Augmented Reality" button to attempt to create an XR session when the user clicks the button.

```javascript
class App {
  ...
  async init() {
    if (!(navigator.xr && XRSession.prototype.requestHitTest)) {
      this.onNoXR();
      return;
    }

    document.querySelector('#enter-ar').addEventListener('click', this.onEnterAR);
  }
}
```

> Even though the WebXR Device API may be supported in a browser, there may not be any support for every session mode. For example, a desktop browser may implement the API, but not have any connected VR or AR hardware to support an experience. Read more about device enumeration in the [WebXR Device API specification](https://immersive-web.github.io/webxr/#deviceenumeration).

We want the output of the session to be displayed on the page, so we must create an [`XRPresentationContext`](https://immersive-web.github.io/webxr/#xrpresentationcontext), similar to how we'd create a [`WebGLRenderingContext`](https://developer.mozilla.org/en-US/docs/Web/API/WebGLRenderingContext) if we were rendering our own WebGL content.

```javascript
class App {
  ...
  async onEnterAR() {
    const outputCanvas = document.createElement('canvas');
    this.ctx = outputCanvas.getContext('xrpresent');
    
    try {
      const session = await navigator.xr.requestSession({
        mode: 'legacy-inline-ar'
      });
      document.body.appendChild(outputCanvas);
      this.onSessionStarted(session);
    } catch (e) {
      this.onNoXR();
    }
  }
}
```

Calling `getContext('xrpresent')` on our canvas returns an `XRPresentationContext`, which is the context that will be displayed on our XR device. Then we request a session via `requestSession()` on the XR device with our desired mode as the `mode` option.

* Upon success, we append the canvas to the DOM and call our `this.onSessionStarted()` function with our newly created [`XRSession`](https://immersive-web.github.io/webxr/#xrsession-interface).
* If the request fails, we fallback and call our function that displays the unsupported browser message, this.onNoXR().

> Important: Before calling `navigator.xr.requestSession()`, we should call `navigator.xr.supportsSessionMode()` with our options to see if our configuration is supported.

Once we have our XRSession, we're ready to set up the rendering with three.js and kick off our animation loop. We create a three.js WebGLRenderer, which contains our second canvas, ensuring alpha and preserveDrawingBuffer are set to true and disabling auto clear. We use the WebGLRenderingContext from three and asynchronously set the compatible XR device. Once the context is considered compatible with the device, we can create an XRWebGLLayer and set it as the XRSession's baseLayer. This tells our session that we want to use this context to draw our scene, to subsequently be displayed on the canvas created in this.init(), composited with our live camera feed.

To render a three.js scene we need three components: a WebGLRenderer to handle the rendering, a scene of objects to render, and a camera to indicate the perspective that scene is rendered from. We'll use a scene created from DemoUtils.createCubeScene() to prepopulate a scene with many cubes floating in space. If you haven't worked with three.js or WebGL before, no worries! If you run into issues rendering, compare your code with the examples.

Before we kick off our rendering loop, we'll need to get an XRFrameOfReference with a 'eye-level' value, indicating that our device is tracking position (as opposed to orientation-only VR experiences like Daydream or GearVR). Once we have our frame of reference, we can use the XRSession's requestAnimationFrame to kick off our rendering loop, similar to window.requestAnimationFrame.

```javascript
class App {
  ...
  async onSessionStarted(session) {
    this.session = session;

    document.body.classList.add('ar');
    
    this.renderer = new THREE.WebGLRenderer({
      alpha: true,
      preserveDrawingBuffer: true,
    });
    this.renderer.autoClear = false;

    this.gl = this.renderer.getContext();
    
    await this.gl.setCompatibleXRDevice(this.session.device);
  
    this.session.baseLayer = new XRWebGLLayer(this.session, this.gl);

    this.scene = DemoUtils.createCubeScene();

    this.camera = new THREE.PerspectiveCamera();
    this.camera.matrixAutoUpdate = false;

    this.frameOfRef = await this.session.requestFrameOfReference('eye-level');
    this.session.requestAnimationFrame(this.onXRFrame);
  }
}
```

The XRSession's requestAnimationFrame allows you to hook into the refresh rate of the native XRDevice. Standard web pages' render loops are designed for 60 FPS, whereas external VR displays may render at 120 FPS. Non-exclusive AR sessions may still only run at 60 FPS, but the device's pose and view information is only accessible within a session's requestAnimationFrame.

On every frame, this.onXRFrame will be called with a timestamp and an XRPresentationFrame. From our frame object, we can get an XRDevicePose, an object, which describes our position and orientation in space, and an array of XRViews, which describes every viewpoint we should render the scene from in order to properly display on the current device.

First, we must fetch the current pose and queue up the next frame's animation by calling session.requestAnimationFrame(this.onXRFrame) before we render. While stereoscopic VR has two views (one for each eye), we will only have one view since we're displaying a fullscreen AR experience. To render we need to loop through each view and set up a camera using the projection matrix it provides and view matrix it gets from the pose. This syncs up the virtual camera's position and orientation with our device's estimated physical position and orientation. Then we can tell our renderer to render our scene with the provided virtual camera.

```javascript
class App {
  ...
  onXRFrame(time, frame) {
    const session = frame.session;
    const pose = frame.getDevicePose(this.frameOfRef);

    session.requestAnimationFrame(this.onXRFrame);

    this.gl.bindFramebuffer(this.gl.FRAMEBUFFER, this.session.baseLayer.framebuffer);

    if (pose) {
      for (let view of frame.views) {
        const viewport = session.baseLayer.getViewport(view);
        this.renderer.setSize(viewport.width, viewport.height);

        this.camera.projectionMatrix.fromArray(view.projectionMatrix);
        const viewMatrix = new THREE.Matrix4().fromArray(pose.getViewMatrix(view));
        this.camera.matrix.getInverse(viewMatrix);
        this.camera.updateMatrixWorld(true);

        this.renderer.clearDepth();

        this.renderer.render(this.scene, this.camera);
      }
    }
  }
}
```

And that's it! We've walked through the code that fetches an XRDevice, creates an XRSession, and renders a scene on each frame, updating our virtual camera's pose with our device's estimated physical pose.

### Test it out
Now that you've looked through the code, let's see our boilerplate in action! We already visited this site during setup, but let's take a look now that we've walked through the code. You should see your camera feed with cubes floating in space whose perspective changes as you move your device. Tracking improves the more you move around, so explore what works for you and your device.

<img src="https://codelabs.developers.google.com/codelabs/ar-with-webxr/img/53edad20e6426c9c.png" width="85%">


If you run into any issues running this app, check the [Introduction]() and [Getting set up]().