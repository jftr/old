---
layout: post
title: Pseudo Element Roundup
tags: [CSS, pseudo-element]
permalink: pseudo-element-roundup
---

[Orig][1]

[1]: http://css-tricks.com/pseudo-element-roundup/
     "A Whole Bunch of Amazing Stuff Pseudo Elements Can Do"

<div class="code-ar"><style>
.post ol.circle-list {
  counter-reset: li;
  list-style: none;
}
.post .circle-list li {
  padding: 1rem 0 2rem 0rem;
  border-bottom: 1px dashed #ccc;
}
.post .circle-list li h3 {
  position: relative;
  margin: 0;
  padding-left: 1.4rem;
  color: #666;
}
.post .circle-list li > h3:before {
  content: counter(li);
  counter-increment: li;
  position: absolute;    
  left: -1rem;
  width: 1.3rem;
  height: 1.3rem;
  border: .12rem solid rgba(0,0,0,.05);
  border-radius: 1rem;
  font: italic bold 1rem/1.3rem Georgia, Serif;
  text-align: center;
  color: #ccc;
  background-color: #f5f5f5;
  -webkit-transition: all .2s ease-out;
          transition: all .2s ease-out;    
}
.post .circle-list li:hover h3:before {
  border-color: rgba(0,0,0,.08);
  border-width: .18rem;
  color: #444;
  -webkit-transform: scale(1.5);
          transform: scale(1.5);
}
.post .circle-list li p {
  margin: 1rem 1.6rem;
}
.post .circle-list pre {
  background-color: #fff; 
  background-image: linear-gradient(#eee .1em, transparent .1em);
  background-size: 100% 1.5em;
}
.post .circle-list blockquote { font-size: 90%; }
.post .circle-list blockquote p:before {
  top: -1rem;
  left: -.5rem;
  font-size: 3rem;
}

.css-shapes { width: 100%; }
.css-shapes .shape {
  float: left;
  width: 25%;
  margin: 1rem 0;
}
.css-shapes style {
  display: block;
  margin: 1rem .5rem 0 0;
  padding: 0.1rem;
  white-space: pre;
  background: #4b4b4b;
  font: 0.6rem Helvetica Arial;
  color: white;
  text-shadow: 0 1px 0 rgba(0,0,0,0.2);
  outline-color: #686868;
}
.css-shapes style:focus { box-shadow: 0 0 5px rgba(60,60,60,.8); }

.tooltip-sample {
  display: -webkit-box;
  display: -webkit-flex;
  display: -ms-flexbox;
  display: flex;
  margin: 1rem 2rem 0;
}
.tooltip-sample a {
  -webkit-flex: auto;
      -ms-flex: auto;
          flex: auto;
}
[data-hint] {
  position: relative;
  display: inline-block;
 }

[data-hint]:before, [data-hint]:after {
  position: absolute;
  visibility: hidden;
  opacity: 0;
  z-index: 2;
  pointer-events: none;
  -webkit-transition: 0.3s ease;
          transition: 0.3s ease;
}
[data-hint]:hover:before, [data-hint]:hover:after {
  visibility: visible;
  opacity: 1;
}
[data-hint]:hover:before, [data-hint]:hover:after {
  -webkit-transition-delay: 100ms;
  transition-delay: 100ms;
}
[data-hint]:before {
  content: '';
  position: absolute;
  background: transparent;
  border: 6px solid transparent;
  z-index: 3;
}
[data-hint]:after {
  content: attr(data-hint);
  background: #383838;
  color: white;
  padding: 8px 10px;
  font-size: 12px;
  line-height: 12px;
  white-space: nowrap;
}

.hint-top:before { border-top-color: #383838; }
.hint-bottom:before { border-bottom-color: #383838; }
.hint-left:before { border-left-color: #383838; }
.hint-right:before { border-right-color: #383838; }

.hint-top:before { margin-bottom: -12px; }
.hint-top:after { margin-left: -18px; }
.hint-top:before, .hint-top:after { bottom: 100%; left: 30%; }
.hint-top:hover:after, .hint-top:hover:before {
  -webkit-transform: translateY(-8px);
      -ms-transform: translateY(-8px);
          transform: translateY(-8px);
}

.hint-bottom:before { margin-top: -12px; }
.hint-bottom:after { margin-left: -18px; }
.hint-bottom:before, .hint-bottom:after { top: 100%; left: 50%; }
.hint-bottom:hover:after, .hint-bottom:hover:before {
  -webkit-transform: translateY(8px);
      -ms-transform: translateY(8px);
          transform: translateY(8px);
}

.hint-right:before { margin-left: -12px; margin-bottom: -6px; }
.hint-right:after { margin-bottom: -14px; }
.hint-right:before, .hint-right:after { left: 60%; bottom: 50%; }
.hint-right:hover:after, .hint-right:hover:before {
  -webkit-transform: translateX(8px);
      -ms-transform: translateX(8px);
          transform: translateX(8px);
}

.hint-left:before { margin-right: -12px; margin-bottom: -6px; }
.hint-left:after { margin-bottom: -14px; }
.hint-left:before, .hint-left:after { right: 100%; bottom: 50%; }
.hint-left:hover:after, .hint-left:hover:before {
  -webkit-transform: translateX(-8px);
      -ms-transform: translateX(-8px);
          transform: translateX(-8px);
}

.hint-e:after { background-color: #b9151b; text-shadow: 0 -1px 0px #592726; }
.hint-e.hint-bottom:before { border-bottom-color: #b9151b; }
.hint-w:after { background-color: #f5e625; text-shadow: 0 -1px 0px #b98429; }
.hint-w.hint-top:before { border-top-color: #f5e625; }
.hint-i:after { background-color: #75caeb; text-shadow: 0 -1px 0px #193b4d; }
.hint-i.hint-left:before { border-left-color: #75caeb; }
.hint-s:after { background-color: #458746; text-shadow: 0 -1px 0px #1a321a; }
.hint-s.hint-right:before { border-right-color: #458746; }

.typographic-flourishes { margin: 1rem auto; text-align: center; }
.typographic-flourishes a { position: relative; font-size: 2rem; color: #333; }
.typographic-flourishes a:hover { color: #333 !important; }
.typographic-flourishes a:before {
  content: "";
  position: absolute;
  width: 100%;
  height: 2px;
  bottom: 0;
  left: 0;
  background-color: #000;
  visibility: hidden;
  -webkit-transform: scaleX(0);
      -ms-transform: scaleX(0);
          transform: scaleX(0);
  -webkit-transition: all 0.5s ease-in-out 0s;
          transition: all 0.5s ease-in-out 0s;
}
.typographic-flourishes a:hover:before {
  visibility: visible;
  -webkit-transform: scaleX(1);
      -ms-transform: scaleX(1);
          transform: scaleX(1);
}</style></div>

<ol class="circle-list">
  <li><h3>Clear floats</h3>

{% highlight js %}
/**
 * For modern browsers
 * 1. The space content is one way to avoid an Opera bug when the
 *    contenteditable attribute is included anywhere else in the document.
 *    Otherwise it causes space to appear at the top and bottom of elements
 *    that are clearfixed.
 * 2. The use of `table` rather than `block` is only necessary if using
 *    `:before` to contain the top-margins of child elements.
 */
.cf:before,
.cf:after {
    content: " "; /* 1 */
    display: table; /* 2 */
}

.cf:after {
    clear: both;
}

/**
 * For IE 6/7 only
 * Include this rule to trigger hasLayout and contain floats.
 */
.cf {
    *zoom: 1;
}
{% endhighlight %}

<p>Ref: <a href="http://nicolasgallagher.com/micro-clearfix-hack/">http://nicolasgallagher.com/micro-clearfix-hack/</a></p>
  </li>
  <li><h3>Multiple background canvases</h3>
<blockquote><p>Because you can absolutely position pseudo elements relative to their parent element, you can think of them as two extra layers to play with for every element.</p></blockquote>
  </li>
  <li><h3>Make shapes with a single element</h3>
<div class="css-shapes cf">
  <div class="shape">
    <div id="triangle-left"></div>
    <style contenteditable="">#triangle-left {
  width: 0;
  height: 0;
  border-top: 50px solid transparent;
  border-right: 100px solid grey;
  border-bottom: 50px solid transparent;
}</style>
  </div>
  <div class="shape">
    <div id="triangle-right"></div>
    <style contenteditable="">#triangle-right {
  width: 0;
  height: 0;
  border-top: 50px solid transparent;
  border-left: 100px solid grey;
  border-bottom: 50px solid transparent;
}</style>
  </div>
  <div class="shape">
    <div id="triangle-topleft"></div>
    <style contenteditable="">#triangle-topleft {
  width: 0;
  height: 0;
  border-top: 100px solid grey;
  border-right: 100px solid transparent;
}</style>
  </div>
  <div class="shape">
    <div id="triangle-bottomright"></div>
    <style contenteditable="">#triangle-bottomright {
  width: 0;
  height: 0;
  border-bottom: 100px solid grey;
  border-left: 100px solid transparent;
}</style>
  </div>
</div>
<p> More: <a href="http://css-tricks.com/examples/ShapesOfCSS/">http://css-tricks.com/examples/ShapesOfCSS/</a></p> 
  </li>
  <li><h3>Label blocks of code with the language it is in</h3>
    <p><code>content: attr(href);</code></p>
    <p><code>data-* attributes</code></p>
  </li>
  <li><h3>Tooltips</h3>
    <div class="tooltip-sample">
      <a href="javascript:void(0)" class="hint-right hint-s" data-hint="...Right">Hint-Right</a>
      <a href="javascript:void(0)" class="hint-left hint-i" data-hint="...Left">Hint-Left</a>
      <a href="javascript:void(0)" class="hint-top hint-w" data-hint="...Top">Hint-Top</a>
      <a href="javascript:void(0)" class="hint-bottom hint-e" data-hint="...Bottom">Hint-Bottom</a>
    </div>
    <p>More: <a href="http://kushagragour.in/lab/hint/">http://kushagragour.in/lab/hint/</a></p>
  </li>
  <li><h3>(Font) Icons</h3>
    <p>More: <a href="http://fontawesome.io/icons/">http://fontawesome.io/icons/</a></p>
  </li>
  <li><h3>Apply typographic flourishes</h3>
    <div class="typographic-flourishes">
      <a>Typographic Flourishes</a>
    </div>
  </li>
</ol>

### Other Examples:

- [Create a body border](http://css-tricks.com/body-border/)
- [Create full browser width bars](http://css-tricks.com/full-browser-width-bars/)
- [Style a header like a three dimensional ribbon](http://css-tricks.com/snippets/css/ribbon/)
- [Make a gleaming button](http://jsfiddle.net/AntonTrollback/nqQc7/)
- [Simulate float: center;](http://css-tricks.com/float-center/)
- [Fade out a page when a particular link is rolled over](http://codepen.io/css-tricks/pen/dxyfA)
- [Be applied or removed conditionally via media queries](http://css-tricks.com/css-media-queries/)
- [Make data tables responsive](http://css-tricks.com/responsive-data-tables/)
- [Style the numbers in ordered lists](http://codeitdown.com/demos/ol-css-styles)
