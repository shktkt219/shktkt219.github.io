---
layout: post
title:  "Animated "x" icon for navbar-toggle"
date:   2017-03-06 04:11:11 +0000
---


When I saw some homepages for my project, I found an awesome transition from the hamburger icon (when the menu is collapsed) to that "x" (when the mobile menu has been expanded). Especially, I looked the homepage of [Novus](https://www.novus.com/), they have a nice transition animation.

I tried to use this animation to my resposive homepage, and I found some awesome tutorials.

* https://julienmelissas.com/animated-x-icon-for-the-bootstrap-navbar-toggle/
* https://sarasoueidan.com/blog/navicon-transformicons/

I wanted to make the animation with CSS. Ok, let's try. 

first, I added some bar classes: top-bar, middle-bar, bottom-bar to distinguish the bars from each other. 

```

<button type="button" class="navbar-toggle collapsed" data-toggle="collapse" data-target="#bs-example-navbar-collapse-1" aria-expanded="false" aria-controls="navbar">
              <span class="sr-only">Toggle navigation</span>
              <span class="icon-bar top-bar"></span>
              <span class="icon-bar middle-bar"></span>
              <span class="icon-bar bottom-bar"></span>
            </button>

```

The way of animating should work like this:

* the right side of the top bar animates down
* the middle bar fades out
* the right side of the bottom bar animates up

Next, I added some CSS styles to help define some of the animation rules. Also added extra effect to hover of navbar-toggle, icon-bar will be gray (#727171) color.

```

.navbar-toggle:hover .icon-bar {
  background-color: #727171;
}

.navbar-toggle .icon-bar {
   transition: all 0.2s;
}

.navbar-toggle .icon-bar.top-bar {
  transform: rotate(45deg);
  transform-origin: 10% 10%;
}

.navbar-toggle .icon-bar.middle-bar {
  opacity: 0;
}

.navbar-toggle .icon-bar.bottom-bar {
  transform: rotate(-45deg);
  transform-origin: 20% 120%;
}


```

Now, my hamburger icon is "X"!! How does it fix? I should add "default" hamburger styles.


```

.navbar-toggle.collapsed .icon-bar.top-bar, .navbar-toggle.collapsed .icon-bar.bottom-bar {
  transform: rotate(0);
}

.navbar-toggle.collapsed .icon-bar.middle-bar {
  opacity: 1;
}

```

Yes! I made it! 

![](http://i.imgur.com/r2QOsas.gif)




