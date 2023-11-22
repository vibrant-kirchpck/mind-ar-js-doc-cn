---
id: events-handling
title: Events Handling
sidebar_label: Events Handling
---

import {customFields} from '/docusaurus.config.js';
import useBaseUrl from '@docusaurus/useBaseUrl';

This example demonstrates how to handle events from MindAR engine. It also explains how to programmatically control the lifecycle of AR engine, including start, stop and switching camera.

The full source code is attached first and we will go through them one by one.

## Try it out
<a href={useBaseUrl('/face-tracking-samples/events.html')} target="_blank">Live Demo</a>

## Source

<code>
{`
<html>
  <head>
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <script src="https://aframe.io/releases/1.4.2/aframe.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/mind-ar@${customFields.libVersion}/dist/mindar-face-aframe.prod.js"></script>
    <script>
      document.addEventListener("DOMContentLoaded", function() {
	const sceneEl = document.querySelector('a-scene');
  let arSystem;
  sceneEl.addEventListener('loaded', function () {
	  arSystem = sceneEl.systems["mindar-face-system"];
	});
	const startButton = document.querySelector("#example-start-button");
	const stopButton = document.querySelector("#example-stop-button");
	const switchCameraButton = document.querySelector("#example-switch-camera-button");
	// arReady event triggered when ready
        sceneEl.addEventListener("arReady", (event) => {
	  console.log("ar ready");
        });
	// detect target found
        sceneEl.addEventListener("targetFound", event => {
          console.log("target found");
        });
	// detect target lost
        sceneEl.addEventListener("targetLost", event => {
          console.log("target lost");
        });
	// arError event triggered when something went wrong. Mostly browser compatbility issue
        sceneEl.addEventListener("arError", (event) => {
	  console.log("ar error");
        });
	startButton.addEventListener('click', () => {
	  arSystem.start(); // start AR 
        });
	stopButton.addEventListener('click', () => {
	  arSystem.stop(); // stop
	});
	switchCameraButton.addEventListener('click', () => {
	  arSystem.switchCamera();
	});
      });
    </script>
    <style>
      body {
        margin: 0;
      }
      .example-container {
        overflow: hidden;
        position: absolute;
        width: 100%;
        height: 100%;
      }
      #example-control-overlay {
	position: fixed;
	top: 0;
	right: 0;
	background: none;
	height: 30px;
	z-index: 2;
      }
      #example-control-overlay >div {
	position: absolute;
	right: 0;
      }
    </style>
  </head>
  <body>
    <div class="example-container">
      <div id="example-control-overlay" class="overlay">
	<div>
	  <button id="example-start-button">Start</button>
	  <button id="example-stop-button">Stop</button>
	  <button id="example-switch-camera-button">Switch Camera</button>
	</div>
      </div>
      <a-scene mindar-face="autoStart: false" embedded color-space="sRGB" renderer="colorManagement: true, physicallyCorrectLights" vr-mode-ui="enabled: false" device-orientation-permission-ui="enabled: false">
        <a-camera active="false" position="0 0 0" look-controls="false"></a-camera>
        <a-entity mindar-face-target="anchorIndex: 1">
	  <a-sphere color="green" radius="0.1"></a-sphere>
        </a-entity>
      </a-scene>
    </div>
  </body>
</html>
`}
</code>

## arSystem

The first thing to introduce is the `arSystem` component. It's embedded inside `a-scene` and you can get the object by the following:

```
const sceneEl = document.querySelector('a-scene');
const arSystem = sceneEl.systems["mindar-face-system"];
```

`arSystem` provides a few api call to control the engine lifecycle 

```
arSystem.start(); // start the engine 
arSystem.stop(); // stop the engine
arSystem.switchCamera(); // switch between front/back cameras
```

By default, AR engine will start immediately, but you can disable the auto start by giving a param `autoStart: false` inside `<a-scene>`

## Events

MindAR will fire the events when the followings happen:

### `arReady`
After `arSystem.start()`, or autostart, AR engine needs to boot up, when it's ready, this event will be fired up. You can listen to this event through the scene element

```
const sceneEl = document.querySelector('a-scene');
sceneEl.addEventListener("arReady", (event) => {
  // console.log("MindAR is ready")
});
```

### `arError`
Sometimes, AR engine might be failed to start. There could be many reasons, but one most likely reason is camera failed to start. When this happens, this event will be fired up.

```
const sceneEl = document.querySelector('a-scene');
sceneEl.addEventListener("arError", (event) => {
  // console.log("MindAR failed to start")
});
```

### `targetFound` and `targetLost`
This events are fired up when a face is detected/lost. You can listen to these events through the `<a-entity>`

```
sceneEl.addEventListener("targetFound", event => {
  console.log("target found");
});

sceneEl.addEventListener("targetLost", event => {
  console.log("target lost");
});

```
