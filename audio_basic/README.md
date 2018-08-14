
# Basic Audio Workshop

In this workshop you will learn how to add audio effects to your VR scene.

Start by remixing [this glitch project](https://webxr-workshop-audio.glitch.me). This project has a cube in it. Our goal is to play a rain sound, then make a clang noise whenever someone clicks on the cube.

First look at the source in `index.html`.  Notice that the scene has a section in it called `a-assets`.  This is the place where we store everything the scene will need to run; including images, sounds, and 3D models. The assets section already has two sounds in it: `clang` and `rain`.

## background sounds

To play the rain sound attach it to the scene. Add the code `sound=" src: #rain; "` to the scene like this

```html
    <a-scene sound="src: #rain;">
```

Whenever you use a word prefixed with the `#` symbol it becomes a *reference by ID*.  In this case we are telling the `sound` component on the scene to look up an asset with an id of `rain`.  In the assets section you can see that the `rain.mp3` file does have an `id` attribute of `rain`.

If you preview your app the scene will open, but you won't hear the sound. Why? Because we haven't made it play!  Add `autoplay:true` to make it automatically play when the scene loads.

```html
<a-scene sound="src: #rain; autoplay:true;">
```

There we go. Now the sound will play for a few seconds. However, we don't want it to just play once. The rain is meant to be the background noise of our scene. It should play constantly. Let's make it loop forever by adding `loop:true;`

```html
    <a-scene sound="src: #rain; autoplay:true; loop:true;">
```

Great. Now our scene has a constant background sound. Let's shift to the cube now.

## Click Events

To play the clang sound first we must attach the sound to the cube, just as we attached the rain to the scene. Add a sound component to the cube referencing the clang sound: `sound="src:#clang"`. Now the cube code will look like this:

```html
<a-box
     id="box"
     position="-1 0.5 -3"
     rotation="0 45 0"
     color="orange"
     sound="src: #clang"
     ></a-box>
```

To make the sound play when the cube is clicked we must add an _event handler_.  

In the `<script>`  section near the bottom there are two shorthand functions called `$` and `on` to make your code simpler.  The `$` function searches the whole page for an element matching a selector.  Searching for `#box` will return an element with an `id` of `box`. (other CSS selectors are supported as well).  The `on` function will attach an event handler. so `on(box,'click', a_function)`  is just shorthand for the longer `box.addEventListener('click',a_function)` function.

```javascript
const $ = (sel) => document.querySelector(sel)
const on = (el,type,cb) => el.addEventListener(type,cb)
```

Below those two functions create a variable `box` to hold a reference to the box.

```javascript
const box = $("#box")
```

Now add a click event handler to show a message

```javascript
on(box,'click',(e)=>{
    alert("someone clicked on the box")
})
```

Refresh the preview page and test it. When you move the cursor to the box and click on it, you will see an alert. Note that if you are testing on a desktop you must make the fusing cursor go inside the box, not your mouse cursor. Then click your mouse to trigger the event.

Great. Now we have an event handler that works. Remove the alert and add this code to play the sound.

```javascript
on(box,'click',()=>{
    box.components.sound.playSound()
})
```

You may recall that every element in A-Frame has a set of components. By putting sound="src:#clang" on the box we added a `sound` component. In the event handler you can reference this component with `box.components.sound`.  From there you can call the `playSound` function to play it.

## Positional Audio

In VR sound can be positional.  We attached the clang to the cube, so if the cube is on the right side of the scene it will actually sound _like it comes from the right_. You may need to have stereo headphones on to hear the effect.  Sounds will also be quieter if they come from further away. Try using the arrow keys to back away from the cube, then click on it again. Notice that it sounds quieter.

The rain sound is *not* positional because we attached it to the *scene* which exists everywhere.  Whenever you want sounds to be everywhere attach them to the scene. If you want the sound to be in just one area of your scene then attach it to some object *within* the scene.

One final detail. The rain sound is meant to add mood but it shouldn't overwhelm the clang sound, which is the focus of the user's attention. Turn the volume of the rain down by adding `volume:0.5;`

```html
<a-scene sound="src: #rain; autoplay:true; loop:true; volume:0.5;">
```


## summary

In this lesson you learned how to
* load a sound file with an audio tag in the assets section
* attach a sound to the scene for non-positional audio
* attach a sound to an object for positional audio
* make sounds loop, auto play, and control the volume
* trigger a sound with a click event handler

A few things to look for when choosing sounds for your project

* Does the sound fit the feel of your scene. If this is a futuristic city then synthetic bleeps and bloops would be good.  If it is a medieval meadow then you'll want more natural and organic sounds.
* Should you have music?  Music can make a scene feel fun, but also distracting.  Generally, if you do have music, keep the volume to under 0.5 or else sound effects will get lost. Also make sure the music fits the theme of your scene. 80s power pop wouldn't fit a scary horror adventure.


## Enrichment
You can get more sounds on the website https://freesound.org It contains thousands of great sounds that people around the world have collected. I've spent hours listening to all kinds of cool stuff.  I you want music instead of noises take a look at [Free Music Archive](http://freemusicarchive.org).  Both sites contain files under Creative Commons licenses.

Remember that these aren't just free audio files. These are creations by others artists. Always respect what they ask you to do. For many files the artist just wants you to credit them and provide a link to their site. Others may ask that you use it only for non-commercial projects. Please *always respect the author's wishes*, just like you'd want others to respect your wishes.

Further ideas:
* Make a drum set from cylinders that play different sounds when you click on them.
* Make a set of repeating sounds that turn on when you click them but also auto-loop. This way the player can produce simple music.
