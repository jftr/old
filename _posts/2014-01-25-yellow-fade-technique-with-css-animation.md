---
layout: post
title: YellowFadeTechnique W/ CSS Animation
permalink: yellow-fade-technique
tags: [CSS, animation]
---

Quick fade on target to attract user attention

{% highlight js %}
:target {
    -webkit-animation: target-fade 3s 1;
    -moz-animation: target-fade 3s 1;
}

@-webkit-keyframes target-fade {
    0% { background-color: rgba(0,0,0,.1); }
    100% { background-color: rgba(0,0,0,0); }
}
@keyframes target-fade {
    0% { background-color: rgba(0,0,0,.1); }
    100% { background-color: rgba(0,0,0,0); }
}
{% endhighlight %}
