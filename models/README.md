# GLTF and 3D Models workshop

### Introduction
In this workshop you will learn about what 3D models are, how to use them in your projects, and how to animate them.  

### What are 3D models
You may remember at  the end of the [geometry workshop](link) that we added a model of a duck using the `gltf-model` component. This model is a collection of geometry made up of triangles (a mesh) combined with a texture to 'skin' the model, and any internal animation.  This model is the output of a 3D modeling program like Blender, Maya, or Autocad.

Everything in VR has geometry. This geometry defines the shape. The geometry may seem like an abstract shape such as a sphere or cylinder, but underneath it is made out of triangles because that's all the computer actually knows how to draw.  Using math the aframe components like a-sphere convert an abstract shape into these triangles.  However, some shapes are too complicated to describe with math. Instead these shapes must be drawn using a 3D modeling tool like Maya or Blender. 

3D modeling tools are very powerful, but they require significant experience to use and the files they output often cannot be used directly on the web. Instead we must convert the models from their native formats to something appropriate for the web: GLTF.

## What is GLTF?
GLTF stands for Graphics Language Transmission Format. It is a cross vender format that has become the standard for 3D objects on the web. It is defined by Khronos, the 3d graphics standards organization, in this specification here. this makes GLTF essentially the PNG for 3d models: a universal standard.

A-Frame and ThreeJS support GLTF natively. While some 3D web frameworks support vendor specific model formats like FXB and OBJ, almost every framework supports GLTF as well.  If someone gives you a 3d model to use, ask them to provide it as a GLTF file.

## Using GLTF files
To use GLTF in your A-Frame project simply reference it as an asset in you aframe scene, then attach it to an entity with the gltf-model component. 

Remix a look at [this example glitch project](link). It contains a single entity with a duck model. The duck model is hosted on `https://vr.josh.earth/assets/models/`, where Josh has collected several models for you to use.

Once you have a model in your scene you can do simple animations with it in A-frame with the a-animation.  Try making the duck rotate by adding this code:

```html
<a-entity gltf-model="#duck" position="0 1.5 -4">
  <a-animation 
       attribute="rotation"
       dur="5000"
       to="0 360 0"
       repeat="indefinite"
   ></a-animation>
</a-entity>
```

This will modify the rotation property over time. To make the animation shorter or faster change the duration attribute.

`a-animation` can handle simple animations. For more complex movement, like animating an object along a path, you can use [Tween.js](https://github.com/CreateJS/TweenJS) or an A-Frame component called [aframe-animation-component](https://github.com/ngokevin/kframe/tree/master/components/animation/).  These allow you to chain animations into sequences. However, even these have limits. They can only move or rotate the object as a whole. They cannot move part of an object, say just the arm of a human model. 

Animating just part of the model really requires the animation to be done by the modeling program that created the model. Most of these tools have animation support built in. The person who created the model can add standard animations for actions like run, sit-down, jump, sleep, etc.  Then you can play back these canned animations from within your project using another component: Don McCurdy's [animation-mixer](s) component.

Add this model to your project.
```
      <a-assets>
        <a-asset-item id="cauldron" src="https://vr.josh.earth/assets/models/evil_cauldron/scene.gltf"></a-asset-item>
      </a-assets>
```

Now change the entity to use this new model instead of the duck and set some rotation and position so we can see it properly.
```
      <a-entity
                gltf-model="#cauldron"
                rotation="45 0 0"
                position="1.5 1.0 -2">
      </a-entity>
```

So far so good.  Now load the animation mixer component from frame-extras at the top of your page
```
    <script src="https://cdn.rawgit.com/donmccurdy/aframe-extras/v4.1.2/dist/aframe-extras.min.js"></script>

```

Now add the animation-mixer component to the entity like this
```
      <a-entity
                gltf-model="#cauldron"
                rotation="45 0 0"
                position="1.5 1.0 -2"
                animation-mixer
                >
      </a-entity>
```


Cool! now the *cauldron* is *bubbling*.

What if the model has more than one animation in it? This example, Hero Knight by iamneuron [Hero Knight (animation clips / takes) - Download Free 3D model by iamneuron (@iamneuron) - Sketchfab](https://sketchfab.com/models/30b8847ce37441a2ab7c00b3d5fe4af2). Has several animations for standing, running, slaying, etc.  To choose between them you should use the ‘clip’ property of the animation-mixer component.

So, this is all good and well, but first we need to find out the *names* of the clips in the model.  We need a tool to inspect the model and give us this information. I highly recommend the `gltf-viewer` by Don McCurdy (if you play around with VR on the web for a while you’ll hear Don’s name over and over). 

[glTF Viewer](https://gltf-viewer.donmccurdy.com/)

Download a GLTF file, drag it’s folder to this webpage, then look in the ‘Animation’ section of the controls in the upper right. This will give you the name of each animation and try them out.

![hero knight in the gltf viewer](images/hero-knight.png)

Then 
```
<a-entity
 gltf-model="#knight"
 animation-mixer="clip: run"
>
</a-entity>
```

You can see the [live](example) example here. 

*prove that this works inside of aframe on glitch*

## Modifying GLTFs

GLTFs are an output format, similar to JPG and PNG. They are not meant for editing. If you wanted to edit a photo  you might use Photoshop's native file format, PSD, and only export to JPG when you are done. GLTFs are similar. They are an export format.  That means we can't easily modify them.

There are a few things we can do, though.  We cannot usually change the materials and colors, but we can modify the position, scale and rotate the model.  

Often times a model is not positioned where we want it. You might put the model in the center of your scene, but when you view it the object visually appears to be off to one side. This is because the model was not centered when it was saved.  We can fix this two ways.

First, set the position on the model to translate it back to the center. Then add a wrapper entity and use it to position the object. 
It would look like this:

```
<a-entity position="0 0 0">
    <a-entity model="gltf-model"
     position="-10 0 0"
    >
</a-entity>
```

## Binary GLTFs

GLTF files are not actually single files. Instead they are folders containing a JSON file which is the core of the model, then some images and other files containing geometry, textures, bump maps, etc.  These other files are all referenced with relative URLS, so normally you can dump the folder on a web server, reference the GLTF file directly, and the rest of the resources will be loaded correctly.

However if you are using a CDN instead of your own web server, you may not have control over relative URLs and they will break. This is the case with Glitch. Instead, you can convert the GLTF to a binary format called GLB, which is a single file containing all of the resources. Then this single file can be put on your CDN and reference directly.

To get GLB files you can either export them directly from your 3d modeling program or use a tool to convert GLTFs to GLBs. [This tool](a) is one such tool.

To convert duck/duck.gltf to a GLB file install the tool and run it like this

```
npm install -g gltf-import-export
gltf-import-export duck/scene.gltf --output duck.glb
```

Or you can use this very nice [web-based converter](https://sbtron.github.io/makeglb/) called MakeGLB by SBTron.

Now upload the `duck.glb` file to the `Assets` section of your glitch project and reference it like this.

```
<a-assets>
        <a-asset-item id="imp" 
                      src="https://cdn.glitch.com/fed11bfd-cd74-4d9e-872b-7c001d62f3f0%2Fimp.glb"></a-asset-item>
</a-assets>

      <a-entity
                id="model"
                gltf-model="#imp"
                position="0 1.5 -4"
                >
      </a-entity>



```


*Very important!*  

When you add assets to a glitch project, glitch will append ?numbers to the asset url. This somehow confuses aframe which refuses to load it. Remove the `?asdf` part and it should work fine. Alternatively, add `response-type="array-buffer"` to the `a-asset-item`.

## Converting from other formats.

Converting from other formats is best done with the modeling tool that created the model.  Many modelers can already export GLTF or have plugins which can do it.  For example, this *link* is a plugin for Blender which does it.  If your modeler can't export GLTF directly, or if you don't have a copy of the modeling software that created it,  you may be able to use a command line tool to do it.

 [This tool](https://www.npmjs.com/package/obj2gltf) can convert OBJ files to GLTFs and [this tool](https://www.npmjs.com/package/collada2gltf) can convert Collada files. There is also [this tool](https://github.com/cyrillef/FBX-glTF) which can convert FBX files but I found it to be more difficult to use because it uses native code underneath.
  
  I have tried several of these tools and they do an okay job, but sometimes have errors due to differences between the original and destination formats. If you have the original tool the model was made it, always use it’s GLTF exporter instead, if at all possible.

If you need to verify that the generated GLTF file is correct, you can try out this validator: [glTF Validator](https://github.khronos.org/glTF-Validator/)


There is also a big list of converters and other tools on [the official GLTF page](https://www.khronos.org/gltf/)

## Next Steps

As GLTF becomes more popular it is getting easier and easier to find models in GLTF format.  SketchFab will auto convert all models on their website into GLTFs. Try searching for the [#medievalfantasyscene tag](https://sketchfab.com/tags/medievalfantasyscene) which are models that were entered into Sketchfab's competition last year that Mozilla sponsored.

[Google Poly](https://poly.google.com/) is another source of GLTF files.

I have also found a lot of files, especially ones for games, at [OpenGameArt](https://opengameart.org/) and [Kenney's assets](https://www.kenney.nl/assets). Most of these are not in GLTF format, so you will need to convert them.

Learning to use a 3D modeler program is not trivial, and is way beyond the scope of this workshop. However, you can find some good getting started with [Blender](https://www.blender.org/), an open source 3D modeler.


## Excercises

* position three instances of *this object*. add some animations to make them dance in sync by wobbling side to side.
* find one creative commons licensed model on sketchfab, which has an animation in it. convert it to GLB, then upload it to the assets of your project and use it.

Validate GLTF files with;

[glTF Viewer](https://gltf-viewer.donmccurdy.com/)


convert FXB files from Kenney's Space Kit assets
[Kenney · Space Kit](https://kenney.nl/assets/space-kit)



