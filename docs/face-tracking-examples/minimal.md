---
id: minimal 
title: Minimal
sidebar_label: Minimal 
---

import {customFields} from '/docusaurus.config.js';
import useBaseUrl from '@docusaurus/useBaseUrl';

This is a minimal example. It detects a face and display a green sphere on nose tip. 

We have a step-by-step tutorial in `Quick Start`. If you are new to MindAR, please check that out to understand some basic principles.

![img](/img/demo/face-minimal-demo.png)

## Try it out
<a href={useBaseUrl('/face-tracking-samples/minimal.html')} target="_blank">Live Demo</a>

## Source
<code>
{`
<html>
  <head>
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <script src="https://aframe.io/releases/1.4.2/aframe.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/mind-ar@${customFields.libVersion}/dist/mindar-face-aframe.prod.js"></script>
  </head>

  <body>
    <a-scene mindar-face embedded vr-mode-ui="enabled: false" device-orientation-permission-ui="enabled: false">
      <a-camera active="false" position="0 0 0"></a-camera>
      <a-entity mindar-face-target="anchorIndex: 1">
        <a-sphere color="green" radius="0.1"></a-sphere>
      </a-entity>
    </a-scene>
  </body>
</html>
`}
</code>
