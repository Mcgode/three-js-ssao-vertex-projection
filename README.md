# three-js-ssao-vertex-projection
A demo of SSAO vertex projection in three.js, and its limitations.

## About this

The goal with this repo is to show an approach for mapping a screen space texture to a mesh in three.js. 

We take the example of SSAO, which has an EffectComposer pass implementation which can give nice results in simple scenes, but has issues when having to handle a lit surface.
By doing a projection of the screen space coordinates of a mesh to its vertices in the form of UV coordinates, we can then just pass the AO texture as an AO map, allowing the
integration of the result in the three.js pipeline, allowing for instance the support of lights.

Another benefit of this approach is that it can be included without hacking/modifying three.js code, and only requires you to override the uv2 coordinates as the scene changes 

Multiple example pages are provided in this repo to show the result:
- [Basic SSAO pass](https://github.com) (notice how the pass simply darkens the lit parts, making them dark yellow instead of orange/dark orange)
- [SSAO vertex mapping](https://github.com) (I know there are artifacts when moving the camera, it could be fixed, but this is not a point of focus, since the idea is mainly to show the difference with the SSAOPass when not moving)

## Issues with the approach

Since we're storing screen space coordinates in vertices and using a perspective projection, interpoled uv2 values don't correspond exactly to their screen space fragment 
coordinates. As such, it creates a distortions in the AO mapping, which is barely noticeable when using triangles that take a very small fraction of the screen space (for instance 
by having a high subdivision count) but can lead to visible mapping artifacts when handling rough meshes, like a box:

![Artifact example](https://github.com/Mcgode/three-js-ssao-vertex-projection/blob/master/projection-artifact.png?raw=true)

It can also be viewed [here](https://github.com)
