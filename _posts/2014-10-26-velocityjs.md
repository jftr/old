---
layout: post
title: Velocity.js
tags: [JavaScript, Velocity.js, animation]
permalink: velocity
---

## Basics

### Basics: Arguments

Velocity takes a map of <ins>CSS properties and values</ins> as its *first* argument. An <ins>options object</ins> can *optionally* be passed in as a *second* argument:

{% highlight js %}
$element.velocity({ 
  width: "500px",
  property2: value2
}, {
  /* Velocity's default options */
  duration: 400,
  easing: "swing",
  queue: "",
  begin: undefined,
  progress: undefined,
  complete: undefined,
  display: undefined,
  visibility: undefined,
  loop: false,
  delay: false,
  mobileHA: true
});
{% endhighlight %}

Option defaults can be globally overriden by modifying `$.Velocity.defaults`.

**Comma-Separated**

Instead of passing in an options object, Velocity also accepts jQuery's comma-separated syntax — but only for the ***duration***, ***easing***, and ***complete*** properties: `$element.velocity(propertyMap [, duration] [, easing] [, complete])`.

{% highlight js %}
$element.velocity({ top: 50 }, 1000, function() { alert("Hi"); });
{% endhighlight %}


### Basics: Properties Map

Velocity auto-prefixes properties (e.g. ***transform*** becomes ***webkit-transform*** on WebKit browsers).

Velocity animates <ins>one numeric value</ins> per property. Hence, you can pass in: `{ padding: 1 }` or `{ paddingLeft: 1, paddingRight: 1 }`. But you cannot pass in { padding: "1 1 1 1" } because you're providing multiple numeric values.

However, Velocity includes hooks to break down the following properties that require multiple values: ***textShadow***, ***boxShadow***, ***clip***, ***backgroundPosition***, and ***transformOrigin***. Refer to the [CSS Support](http://julian.com/research/velocity/#cssSupport) dropdown for a full listing of hooks.

Velocity supports the `px`, `em`, `rem`, `%`, `deg`, and `vw/vh` units. If you do not provide a unit, an appropriate one is automatically assigned — usually `px`, but `deg` in the case of ***rotateZ*** for example.

Velocity supports four value operators: `+`, `-`, `*`, and `/`. You may suffix an equals sign onto any one of them to perform relative math operations.

{% highlight js %}
$element.velocity({ 
  top: 50, // Defaults to the px unit type
  left: "50%",
  width: "+=5rem", // Add 5rem to the current rem value
  height: "*=2" // Double the current height
});
{% endhighlight %}

Browser support: rem units are not supported below IE 9. vh/vw units are not supported below IE 9 or below Android 4.4.

-----

## Options

### Option: Duration

Velocity supports durations specified in milliseconds as well as jQuery's named durations: `"slow"`, `"normal"`, and `"fast"`.

{% highlight js %}
$element
  /* Animate the width property. */
  .velocity({ width: 75 })
  /* Then, when finished, animate the height property. */
  .velocity({ height: 0 });
{% endhighlight %}


### Option: Easing

You can either pass in the name of a packaged easing (e.g. `"ease-out"` or `"easeInSine"`), or you can pass in one of the array types:

{% highlight js %}
/* Use one of the jQuery UI easings. */
$element.velocity({ width: 50 }, "easeInSine");
/* Use a custom bezier curve. */
$element.velocity({ width: 50 }, [ 0.17, 0.67, 0.83, 0.67 ]);
/* Use spring physics. */
$element.velocity({ width: 50 }, [ 250, 15 ]);
/* Use step easing. */
$element.velocity({ width: 50 }, [ 8 ]);
{% endhighlight %}

Easings can also optionally be declared on a per-property basis by passing in an two-item array as the property's value. The first item is the standard end value, and the second item is the easing type:

{% highlight js %}
$element.velocity({
  borderBottomWidth: [ "2px", "spring" ], // Uses "spring"
  width: [ "100px", [ 250, 15 ] ], // Uses custom spring physics
  height: "100px" // Defaults to easeInSine, the call's default easing
}, { 
  easing: "easeInSine" // Default easing
});
{% endhighlight %}

As with jQuery's ***$.animate()***, `"swing"` is Velocity's default easing type.


### Option: Queue

You can set ***queue*** to `false` to force a new animation call to immediately run in parallel with any currenty-active animations. 

{% highlight js %}
/* Trigger the first animation (width). */
$element.velocity({ width: "50px" }, { duration: 3000 }); // Runs for 3s
setTimeout(function() {
  /* Will run in parallel starting at the 1500ms mark. */
  $element.velocity({ height: "50px" }, { queue: false, duration: 1500 });
}, 1500);
{% endhighlight %}

Alternatively, a custom queue — a queue that doesn't immediately start — can be created by passing queue the name of your custom queue. You can build multiple custom queues in parallel, then later start them individually via `$element.dequeue("queueName")` (or `Velocity.Utilities.dequeue(element(s), "queueName")` if you're using Velocity without jQuery).

{% highlight js %}
// Create two custom queues.
$element
  .velocity({ translateX: 75 }, { queue: "a" })
  .velocity({ width: 50 }, { queue: "b" })
  .velocity({ translateY: 75 }, { queue: "a" })
  .velocity({ height: 0 }, { queue: "b" })
// Later on, dequeue the "a" queue (translations).
setTimeout(function() {
  $element.dequeue("a");
}, 2000);
// Even later, dequeue the "b" queue (dimensions).
setTimeout(function() {
  $element.dequeue("b");
}, 4000);
{% endhighlight %}


### Option: Begin & Complete

Pass the ***begin*** option a function to be triggered *prior to the start* of the animation.

Pass the ***complete*** option a function to be triggered *once the animation has finished*.

The callback is executed *once* per call, even if multiple elements are being animated. Further, if a call is looped, the callback only fires once.

The callback is passed the entire raw DOM (not jQuery) element array as both its context and its first argument. To access these elements individually, you must iterate over the array using jQuery's ***$.each()*** or JavaScript's native ***.forEach()***.

{% highlight js %}
$element.velocity({
  scale: 1.5
}, {
  duration: 2000,
  begin: function() { 
    alert("This is called before the animation begins.");
  }
});
{% endhighlight %}

{% highlight js %}
$element.velocity({
  scale: 1.5
}, { 
  duration: 2000,
  begin: function() { 
    alert("Done animating the scale property.");
  }
});
{% endhighlight %}


### Option: Progress

Pass the ***progress*** option a callback function to be repeatedly triggered througout the duration of the animation.

The callback is passed the entire raw DOM (not jQuery) element array as both its context and its first argument. To access these elements individually, you must iterate over the array using jQuery's ***$.each()*** or JavaScript's native ***.forEach()***. Further, it's passed `percentComplete (decimal value)`, `timeRemaining (in ms)`, and `timeStart (Unix time)`:

[Demo](http://codepen.io/julianshapiro/pen/Jktjq)


### Option: Loop

Instead of passing in an integer, you can pass in `true` to trigger infinite looping:

{% highlight js %}
$element.velocity({ height: "10em" }, { loop: true });
{% endhighlight %}

Infinite loops never return promises, ignore the ***complete*** callback, and cannot be used with `queue: false`. Further, be sure to stop one infinite loop before triggering another infinite loop on the same element. (A loop can be stopped with the `Stop` command.)


### Option: Delay

You can set the delay option alongside the loop option to create a pause between loop alternations.


### Option: Display & Visibility

Velocity's ***display*** and ***visibility*** options correspond directly to their CSS counterparts, and therefore accept the same values: ***display*** accepts `"inline"`, `"inline-block"`, `"block"`, `"flex"`, `""` (empty quotes to remove the property altogether), and whatever else the browser natively supports. ***visibility*** accepts `"hidden"`, `"visible"`, `"collapse"`, and `""` (empty quotes to remove the property altogether). For a comparison between the behaviors of ***display***, ***visibility***, and ***opacity***, see [this post](http://stackoverflow.com/a/273076/3329855).

Exclusive to Velocity, the ***display*** option also takes a non-standard value of `"auto"`, which instructs Velocity to set the element to its native display value. For example, <ins>anchor elements default to "inline" whereas div elements default to "block"</ins>.

When the ***display*** option is set to `"none"` (or when ***visibility*** is set to `"hidden"`), the CSS property is set accordingly *after* the animation has completed. This works to hide the element upon animation completion, and is useful when animating an element's opacity down to 0. (When these options are used with the ***loop*** option, they are set at the end of the final loop iteration.)

Here, an element is removed from the page's flow once it has faded out. This replaces jQuery's ***$.fadeOut()*** function:

{% highlight js %}
/* Animate down to zero then set display to "none". */
$element.velocity({ opacity: 0 }, { display: "none" });
{% endhighlight %}

Conversely, when ***display***/***visibility*** is set to a value other than `"none"`/`"hidden"`, such as `"block"`/`"visible"`, the corresponding value is set *before* the animation begins so that the element is visible throughout the duration of the animation.

(Further, if opacity is simultaneously animated, it'll be forced to a start value of 0. This is useful for fading an element back into view.)

Here, ***display*** is set to `"block"` *before* the element begins fading:

{% highlight js %}
/* Set display to "block" then animate from opacity: 0. */
$element.velocity({ opacity: 1 }, { display: "block" });
{% endhighlight %}

Note: The display and visibility options are ignored when used with the `Reverse` command.


### Option: mobileHA

mobileHA stands for mobile hardware acceleration. When set to true — its default value — Velocity animations are automatically hardware accelerated on mobile devices.

turn mobileHA off by setting it to false:

{% highlight js %}
$element.velocity(propertiesMap, { mobileHA: false });
{% endhighlight %}

-----

## Commands

### Command: Fade & Slide

`"fadeIn"` and `"fadeOut"`

`"slideUp"` and `"slideDown"`


### Command: Scroll

`"scroll"`

To scroll toward an element that's <ins>inside a containing element with scrollbars</ins>, the scroll command uniquely takes an optional ***container*** option, which accepts either a jQuery object or a raw DOM element. The container element must have its CSS ***position*** property set to either `relative`, `absolute`, or `fixed` — static will not work:

{% highlight js %}
/* Scroll $element into view of $("#container"). */
$element.velocity("scroll", { container: $("#container") });
{% endhighlight %}

By default, scrolling occurs on the Y axis. Pass in the `axis: "x"` option to scroll horizontally instead:

{% highlight js %}
/* Scroll the browser to the LEFT edge of the targeted div. */
$element.velocity("scroll", { axis: "x" });
{% endhighlight %}

Scroll also uniquely takes an *offset* option, specified in pixels, which offsets the target scroll position:

{% highlight js %}
$element
  /* Then scroll to a position 250 pixels BELOW the div. */
  .velocity("scroll", { duration: 750, offset: 250 })
  /* Scroll to a position 50 pixels ABOVE the div. */
  .velocity("scroll", { duration: 750, offset: -50 });
{% endhighlight %}

Alternatively, to scroll the entire browser window by an *arbitrary* amount (instead of to the edge of an element), simply target the ***html*** element and use a custom ***offset*** value. Optionally, additionally pass in `mobileHA: false` to avoid potential flickering issues on iOS:

{% highlight js %}
/* Scroll the whole page to an arbitrary value. */
$("html").velocity("scroll", { offset: "750px", mobileHA: false });
{% endhighlight %}


### Command: Stop

To immediately stop all current Velocity calls on an element, pass in `"stop"` as Velocity's first argument. The next Velocity call in the element's animation queue immediately starts. If you're stopping a custom queue, pass the queue's name as the second parameter.

{% highlight js %}
$element.velocity("stop"); // Normal stop
$element.velocity("stop", "myQueue"); // Stop a custom queue
{% endhighlight %}

When a call is stopped, its ***complete*** callback and `display: none` toggling are skipped.

A common pattern is to stop a call then animate back to its starting values using Velocity's `reverse` command:

{% highlight js %}
/* Prior Velocity call. */
$element.velocity({ opacity: 0 });
/* Later, midway through the call... */
$element
  /* Stop animating opacity. */
  .velocity("stop")
  /* Animate opacity back to 1. */
  .velocity("reverse");
{% endhighlight %}

Important: Note that a looped Velocity call is actually a series of Velocity calls chained together. Accordingly, to completely stop a looping animation, you must pass in the second parameter to clear the element's remaining calls. Similarly, in order to fully stop a UI pack effect (which can consist of multiple Velocity calls), you will need to clear the entirety of the element's pending animation queue.

**Stopping Multi-Element Calls**

Because Velocity uses call-based tweening, when `stop` is called on an element, it is specifically the call responsible for that element's current animation that is stopped. Thus, <ins>if other elements were also targeted by that same call, they will also be stopped</ins>.

If you want *per-element* stop control in a multi-element animation, <ins>perform the animation by calling Velocity once on each element</ins>.

**Clearing the Animation Queue**

When a call is stopped, the next Velocity call in the element's queue chain fires immediately. Alternatively, you may pass in `true` (or the name of a custom queue) to clear all of the element's remaining queued calls:

{% highlight js %}
/* Prior Velocity calls. */
$element
  .velocity({ width: 100 }, 1000)
  .velocity({ height: 200 }, 1000);
/* Called immediately after. */
$element.velocity("stop", true);
{% endhighlight %}

Above, the initial `{ width: 100 }` call will be instantly stopped, and the ensuing `{ height: 200 }` will be removed and skipped entirely.

Important: If you're clearing the remaining queue entries on a call that targeted more than one element, be sure that your `stop` command is applied to the full set of elements that the call initially targeted. Otherwise, some elements will have stuck queues and will ignore further Velocity calls until they are manually dequeued.

### Command: Reverse

To animate an element back to the values prior to its last Velocity call, pass in `"reverse"` as Velocity's *first* argument.

Reverse defaults to the options (duration, easing, etc.) used in the element's previous Velocity call. However, this can be overridden by passing in a new options object:

{% highlight js %}
$element.velocity("reverse", { duration: 2000 });
{% endhighlight %}

The previous call's callback options (***begin*** and ***complete***) are ignored by `reverse`; `reverse` does not re-fire callbacks.

-----

## Features

### Feature: Transforms

To achieve parity with CSS, Velocity uses the ***translateX*** and ***translateY*** property names for transform translations, not X and Y.

All browsers automatically trigger hardware acceleration (HA) when 3D transform properties (translateZ and rotateX/Y) are being animated. The advantages of HA include increased smoothness, while the disadvantages include blurry text and memory consumption. If you want to also force HA for 2D transforms in order to benefit from the increased performance it can provide (at the expense of possible text blurriness), then trigger HA by animating a 3D transform property to a bogus value (such as 0) during your animation:

{% highlight js %}
/* Translate to the right and rotate clockwise. */
$element.velocity({ 
  translateZ: 0, // Force HA by animating a 3D property
  translateX: "200px",
  rotateZ: "45deg"
});
{% endhighlight %}

Browser support: Remember that 3D transforms are not supported below IE 10 and below Android 3.0, and that even 2D transforms aren't supported below IE 9. Also, remember that a perspective must be set on a parent element for 3D transforms to take effect. See MDN's transform guide to learn more.


### Feature: Colors

Velocity supports color animation for the following properties: ***color***, ***backgroundColor***, ***borderColor***, and ***outlineColor***.

You can pass a color property a hex string (rgb, hsla strings, and color keywords are not supported), or you can animate the individual RGB component values of a color property: The color components are Red, Green, Blue, and Alpha, and they range from from 0 to 255. (They may also be passed a percent value.) The fourth component, Alpha (the same thing as opacity), ranges from 0 to 1.


### Feature: SVG


### Feature: Hook

Hooks are the subvalues of multi-value CSS properties. For example, the textShadow CSS property takes a multi-value form of "0px 0px 0px black". Velocity allows you to individually animate each of these subvalues, e.g. ***textShadowX***, ***textShadowY***, and ***textShadowBlur***:

Since it's not possible to individually set these hook values via jQuery's ***$.css()***, Velocity provides a hook() helper function. It features the same API as ***$.css()***, with the modification that it takes an element as its first argument (either a raw DOM node or a jQuery object): `$.Velocity.hook(element, property [, value]);`:

Set a hook value:

{% highlight js %}
$.Velocity.hook($element, "translateX", "500px"); // Must provide unit type
$.Velocity.hook(elementNode, "textShadowBlur", "10px"); // Must provide unit type
{% endhighlight %}

Get a hook value:

{% highlight js %}
$.Velocity.hook($element, "translateX");
$.Velocity.hook(elementNode, "textShadowBlur");
{% endhighlight %}

Note: When using the hook function, you must provide a unit type (e.g. px, deg, etc.) if the CSS property can accept one.


### Feature: Promises

A Velocity call automatically returns a promise object when both A) the utility function is used and B) promise support is detected in the browser. Conversely, promises are never returned when using jQuery's object syntax (e.g. $element.velocity(...)).

{% highlight js %}
/* Using Velocity's utility function... */
$.Velocity.animate(element, { opacity: 0.5 })
  /* Callback to fire once the animation is complete. */
  .then(function(elements) { console.log("Resolved."); })
  /* Callback to fire if an error occurs. */
  .catch(function(error) { console.log("Rejected."); });
{% endhighlight %}


### Feature: Utility Function

Instead of using jQuery's plugin syntax to target jQuery objects, you can use Velocity's utility function to target raw DOM elements:

{% highlight js %}
/* Standard multi-argument syntax. */
var divs = document.getElementsByTagName("div");
$.Velocity(divs, { opacity: 0 }, { duration: 1500 });
/* Alternative single-argument syntax (ideal for CoffeeScript). */
var divs = document.getElementsByTagName("div");
$.Velocity({
  elements: divs,
  properties: { opacity: 0 },
  options: { duration: 1500 }
);
{% endhighlight %}

-----

## Advanced

### Advanced: Value Functions







