---
layout: post
title: Image Comparison Slider W/ CSS
tags: [CSS, image]
permalink: image-comparison-slider
---

[Orig][1]

[1]: http://lea.verou.me/2014/07/image-comparison-slider-with-pure-css/
     "Image comparison slider with pure CSS"

<div class="code-ar"><style>
.image-comparison-slider { position: relative; }
.image-comparison-slider > div {
  position: absolute;
  top: 0; bottom: 0; left: 0;
  width: 25px;
  max-width: 100%;
  overflow: hidden;
  resize: horizontal;
}
.image-comparison-slider > div:before {
  content: '';
  position: absolute;
  right: 0; bottom: 0;
  width: 13px; height: 13px;
  padding: 5px;
  background: linear-gradient(-45deg, white 50%, transparent 0);
  background-clip: content-box;
  cursor: ew-resize;
  -webkit-filter: drop-shadow(0 0 2px black);
  filter: drop-shadow(0 0 2px black);
}
.image-comparison-slider img {
  -webkit-user-select: none;
  user-select: none;
  max-width: 400px;
}</style></div>

<div class="image-comparison-slider">
  <div>
  	<img src="https://s3-us-west-2.amazonaws.com/s.cdpn.io/4273/photoshop-face-before.jpg" />
  </div>
  <img src="https://s3-us-west-2.amazonaws.com/s.cdpn.io/4273/photoshop-face-after.jpg" />
</div>

{% highlight js %}
/**
 * Image slider with pure CSS
 * Original version in http://demosthenes.info/blog/css
 */

.image-slider {
	position:relative;
	display: inline-block;
	line-height: 0;
}

/* Could use a pseudo-element, but they’re currently
   super buggy. See: http://dabblet.com/gist/ab432c3f6a8f672cd077 */
.image-slider > div {
	position: absolute;
	top: 0; bottom: 0; left: 0;
	width: 25px;
	max-width: 100%;
	overflow: hidden;
	resize: horizontal;
}

/* Cross-browser resizer styling */
.image-slider > div:before {
	content: '';
	position: absolute;
	right: 0; bottom: 0;
	width: 13px; height: 13px;
	padding: 5px;
	background: linear-gradient(-45deg, white 50%, transparent 0);
	background-clip: content-box;
	cursor: ew-resize;
	-webkit-filter: drop-shadow(0 0 2px black);
	filter: drop-shadow(0 0 2px black);
}

.image-slider img {
  -webkit-user-select: none;
	user-select: none;
	max-width: 400px;
}
{% endhighlight %}

