WebGL Clustered Deferred and Forward+ Shading
======================

**University of Pennsylvania, CIS 565: GPU Programming and Architecture, Project 5**

* Ricky Rajani
* Tested on: **Google Chrome 62.0.3202** on
  Windows 10, i5-6200U @ 2.30GHz, Intel(R) HD Graphics 520 4173MB (Personal Computer)

This project implements Clustered Deferred and Forward+ Shading using WebGL.

### Live Online

- Num of Lights: 1500
- Light Radius: 3.0

![](img/live.PNG)

### Demo Video/GIF

[![Foo](img/videoScreenshot.PNG)](https://www.youtube.com/watch?v=vU8VylBNE9A&feature=youtu.be)

### Features
- Clustered Forward+
- Clustered Deferred
- Blinn-Phong shading
- Optimizations of g-buffers

Clustered Shading groups samples from a view into clusters. For shading, each cluster is assigned lights that affect the cluster. This provides for better worse case performance with large depth discontinuities. Clustered Forward+ is forward shading with light culling for screen-space tiles using clustered shading. It has a high compute-to-memory ratio. Clustered Deferred consists of two passes: G-buffer pass and lighting pass. All the shading occurs during the lighting pass using clustered shading. The primary advantage of deferred shading is the decoupling of scene geometry from lighting. Only one geometry pass is required and each light is only computed for those pixels that it actually affects. This gives the ability to render many lights in a scene without a significant performance-hit.

### Performance Analysis

Testing number of lights
  - Light's radius	: 3.0
  - Resolution	: 1920 x 1080
  - Cluster Dimension : 16 x 16 x 16
  
[![](img/numLightsGraph.PNG)]

Deferred shading is better for large number of lights. Deferred shading grabs its shading informationg from the closest fragment (from g-buffers), it is faster than Forward+. This advantage comes from deferred only having to shade one fragment per pixel rather than all the fragments associated to each pixel.

Testing resolution
  - Number of Lights : 1000 
  - Light's radius	: 3.0
  - Cluster Dimension : 16 x 16 x 16

[![](img/resolutionGraph.PNG)]

Deferred shading's performance depends more on screen resolution than scene complexity. Here you can see that its efficiency increases as the resolution increases in a scene with 1000 lights.
  
Blinn Phong Shading Model:
  - TODO: Add graph comparing times of lambert and blinn phong on Forward+ and Deferred with 500, 1000, 1500 lights
  - TODO: Add analysis
  
Optimized g-buffer format:
  - Used two rather than four g-buffers
    - Use 2-component normals
    - Reduce number of properties passed via g-buffer by reconstructing world space position
      - G-buffer01: {color.r, color.g, color.b, depth}
      - G-buffer02: {normal.x, normal.y, - , - }
  - TODO: Add graph comparing times of 2 versus 4 g-buffers on Forward+ and Deferred with 500, 1000, 1500 lights

### Credits

* [Three.js](https://github.com/mrdoob/three.js) by [@mrdoob](https://github.com/mrdoob) and contributors
* [stats.js](https://github.com/mrdoob/stats.js) by [@mrdoob](https://github.com/mrdoob) and contributors
* [webgl-debug](https://github.com/KhronosGroup/WebGLDeveloperTools) by Khronos Group Inc.
* [glMatrix](https://github.com/toji/gl-matrix) by [@toji](https://github.com/toji) and contributors
* [minimal-gltf-loader](https://github.com/shrekshao/minimal-gltf-loader) by [@shrekshao](https://github.com/shrekshao)
