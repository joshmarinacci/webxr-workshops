# webxr-workshop

A modular workshop series for teaching WebXR (VR &amp; AR)

WebXR is the (soon to be) standard for Virtual Reality and Augmented Reality on the web. This
workshop is a series of modules which teaches students how to create their own VR and AR
applications, starting with the basics of what HTML is, going through 3D objects, lighting,
materials, physics, and more.

## Notes for teachers

This workshop is designed around a series of modules. You can choose which modules to teach depending on the skill level of your students and the time available for the workshop.
Each module should take around 30 minutes or less to complete, and is built around a particular topic. This workshop uses [A-frame](https://aframe.io/), an open source VR framework, and [Glitch](https://glitch.com/), a free online editor that does not require setting up accounts.

For beginners I suggest starting with the [intro](intro/) workshop first, which explains what HTML is and introduces Glitch, a web-based editor that automatically refreshes the page. If you have
young students who have never worked with code before (say less than 10 years old), then you should
do the module as a class with the students following the teacher live. For more experienced
students they can complete the workshop modules on their own and just come ask for help.


## Available modules

* [Introduction to HTML, WebXR, and A-Frame](intro/)
  * explains how markup in the *input code* becomes elements in the *output page*
  * first VR example
* [Basic Geometry](geometry_basic/)
  * introduces cubes, spheres, cylinders and other primitive solids
  * visualize the underlying mesh of polygons that makes up a shapes
  * first example of using images and full 3d models in VR
* [Basic Audio](audio_basic/)
  * add background music to a scene
  * explore positional audio
  * trigger sounds when clicking on objects
* [Basic Lighting](lighting_basic/)
  * learn how lights affect the color and shade of objects
  * explore directional, ambient, emissive, and point lighting_basic
  * create simple shadows
* [360 Images](360images/)
  * learn what 360 images are
  * learn how to find 360 images on the web and add them to your own projects
  * learn about different kinds of 360 images and 360 cameras
* [Physics](physics/)
  * learn how physics affects a scene
  * explore how different gravity, friction, and bounciness affect the scene
  * build a simple ping pong game with collision tracking for scores


## Coming soon

* Animation and Particle effects
* Working with complex 3D models
* Events and input management
* making custom geometry with code
