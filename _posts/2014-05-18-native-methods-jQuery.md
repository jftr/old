---
layout: post
title: Native Equivalents of jQuery Functions
tags: Javascript jQuery
permalink: native-methods-jQuery
---

[Orig][1]

[1]: http://www.leebrimelow.com/native-methods-jQuery/
     "Native Equivalents of jQuery Functions"
[Related](http://www.sitepoint.com/series/native-javascript-equivalents-of-jquery-methods/ "Native JavaScript Equivalents of jQuery Methods") articles from Sitepoint.

### Selectors

{% highlight js %}
//----Get all divs on page--------- 

  /* jQuery */
  $("div")

  /* native equivalent */
  document.getElementsByTagName("div") 

//----Get all by CSS class--------- 

  /* jQuery */ 
  $(".my-class")

  /* native equivalent */
  document.querySelectorAll(".my-class")

  /* FASTER native equivalent */
  document.getElementsByClassName("my-class") 

//----Get by CSS selector---------- 

  /* jQuery */
  $(".my-class li:first-child")
 
  /* native equivalent */
  document.querySelectorAll(".my-class li:first-child") 

//----Get first by CSS selector----
 
  /* jQuery */
  $(".my-class").get(0)

  /* native equivalent */
  document.querySelector(".my-class")
{% endhighlight %}

### DOM manipulation

{% highlight js %}
//----Append HTML elements----
 
  /* jQuery */
  $(document.body).append("<div id='myDiv'><img src='im.gif'/></div>");
 
  /* CRAPPY native equivalent */
  document.body.innerHTML += "<div id='myDiv'><img src='im.gif'/></div>";
 
  /* MUCH BETTER native equivalent */
  var frag = document.createDocumentFragment();
 
  var myDiv = document.createElement("div");
  myDiv.id = "myDiv";
 
  var im = document.createElement("img");
  im.src = "im.gif";

  myDiv.appendChild(im);
  frag.appendChild(myDiv);
 
  document.body.appendChild(frag);

//----Prepend HTML elements----

  // same as above except for last line
  document.body.insertBefore(frag, document.body.firstChild);
{% endhighlight %}

### CSS classes

{% highlight js %}
// get reference to DOM element
  var el = document.querySelector(".main-content");
 
//----Adding a class------
 
  /* jQuery */
  $(el).addClass("someClass");
 
  /* native equivalent */
  el.classList.add("someClass");
 
//----Removing a class-----
 
  /* jQuery */
  $(el).removeClass("someClass");
 
  /* native equivalent */
  el.classList.remove("someClass");
 
//----Does it have class---
 
  /* jQuery */
  if($(el).hasClass("someClass"))
 
  /* native equivalent */
  if(el.classList.contains("someClass"))
{% endhighlight %}

### Modifying CSS properties

{% highlight js %}
// get reference to a DOM element
  var el = document.querySelector(".main-content");

//----Setting multiple CSS properties----
 
  /* jQuery */
  $(el).css({
    background: "#FF0000",
    "box-shadow": "1px 1px 5px 5px red",
    width: "100px",
    height: "100px",
    display: "block"
  });
 
  /* native equivalent */
  el.style.background = "#FF0000";
  el.style.width = "100px";
  el.style.height = "100px";
  el.style.display = "block";
  el.style.boxShadow = "1px 1px 5px 5px red";
{% endhighlight %}
