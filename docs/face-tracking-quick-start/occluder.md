---
id: occluder 
title: Occluder 
sidebar_label: Occluder 
---

import {customFields} from '/docusaurus.config.js';
import demoImage1 from '@site/static/img/demo/face-occluder1-demo.png'
import demoImage2 from '@site/static/img/demo/face-occluder2-demo.png'

## Depth Problem

If you turn your head a bit, you will probably notice a problem (the arm of the glass should be invisible since it's behind the head):

<img src={demoImage1} width="300" />

It's a common problem in augmented reality application. AR application does not alter the video. Instead it is just merely drawing another layer on top. Therefore, none of the video content can obscure our drawing layer (i.e. augmented realty objects). 

## Occluder

To solve this issue, we will need to add an arbitrary 3D head-like object in the scene. But unlike a regular 3D object, this arbitrary head has two special properties. First, obviously it needs to be transparent. Second, despite being transparent, it still need to cover every behind. Normally, we call this kind of special 3D objects as occluders.

Adding a occluder object is very similar to adding a regular object. In MindAR, you just need to add a property `mindar-face-occluder` to the entity.

<code>
{`
<a-assets>
  <a-asset-item id="headModel" src="https://cdn.jsdelivr.net/gh/hiukim/mind-ar-js@${customFields.libVersion}/examples/face-tracking/assets/sparkar/headOccluder.glb"></a-asset-item>
</a-assets>
...
<a-entity mindar-face-target="anchorIndex: 168">
  <a-gltf-model mindar-face-occluder position="0 -0.3 0.15"rotation="0 0 0" scale="0.065 0.065 0.065" src="#headModel"></a-gltf-model>
</a-entity>
`}
</code>

## Putting it together

Putting it together, your new html page should be like this:

<code>
{`
<html>
  <head>
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <script src="https://aframe.io/releases/1.4.2/aframe.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/mind-ar@${customFields.libVersion}/dist/mindar-face-aframe.prod.js"></script>
  </head>

  <body>
    <a-scene mindar-face embedded color-space="sRGB" renderer="colorManagement: true, physicallyCorrectLights" vr-mode-ui="enabled: false" device-orientation-permission-ui="enabled: false">
      <a-assets>
	<a-asset-item id="glassesModel" src="https://cdn.jsdelivr.net/gh/hiukim/mind-ar-js@${customFields.libVersion}/examples/face-tracking/assets/glasses/scene.gltf"></a-asset-item>
	<a-asset-item id="headModel" src="https://cdn.jsdelivr.net/gh/hiukim/mind-ar-js@${customFields.libVersion}/examples/face-tracking/assets/sparkar/headOccluder.glb"></a-asset-item>
      </a-assets>\n
      <a-camera active="false" position="0 0 0"></a-camera>
      <a-entity mindar-face-target="anchorIndex: 168">
        <a-gltf-model mindar-face-occluder position="0 -0.3 0.15"rotation="0 0 0" scale="0.065 0.065 0.065" src="#headModel"></a-gltf-model>
      </a-entity>
      <a-entity mindar-face-target="anchorIndex: 168">
        <a-gltf-model rotation="0 0 0" position="0 0 0" scale="0.01 0.01 0.01" src="#glassesModel"></a-gltf-model>
      </a-entity>
    </a-scene>
  </body>
</html>
`}
</code>

and when you run the app again, the arm of the glasses will be somehow hidden. 

<img src={demoImage2} width="300" />

## Limitations

One major limitation of occluder is that the arbitrary head is a predefined 3D model, and the shape won't fit perfectly with the persons' head in the video.
