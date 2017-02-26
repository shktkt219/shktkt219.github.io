---
layout: post
title:  "How to close Bootstrap collapsed menu"
date:   2017-02-26 19:04:27 +0000
---


I'm creating a website with using bootstrap. 

```

<nav class="navbar navbar-default navbar-fixed-top">
        <div class="container">
          <div class="navbar-header">
            <button type="button" class="navbar-toggle collapsed" data-toggle="collapse" data-target="#bs-example-navbar-collapse-1" aria-expanded="false">
              <span class="sr-only">Toggle navigation</span>
              <span class="icon-bar"></span>
              <span class="icon-bar"></span>
              <span class="icon-bar"></span>
            </button>
            <a class="navbar-brand" href="#home">
              <img class="logo" src="images/top_logo.gif" alt="Capital Management">
            </a>
          </div>
          <div class="collapse navbar-collapse" id="bs-example-navbar-collapse-1">
            <ul class="nav navbar-nav navbar-right">
              <li><a href="#home">Home</a></li>
              <li><a href="#about">About</a></li>
              <li><a href="#team">Team</a></li>
              <li><a href="#contact">Contact</a></li>
            </ul>
          </div><!-- /.navbar-collapse -->
        </div><!-- /.container -->
      </nav>

```

It collapses normally on smaller devices. Clicking on the menu button expands the menu but if I click on a menu link, the menu stays open like this:

![](http://i.imgur.com/jd5W7Yc.gif)

What do we have to do to close it again?

Adding the jQuery element `hide` to `.navbar-collapse`, we can close `.navbar-collapse` menu.


*# script.js*
```

$(document).ready(function(){
  $(".navbar-nav li a").click(function(event){
    $(".navbar-collapse").collapse('hide');
  });
});

```

We should add script.js the end of the body tag in the index.html.

*index.html*
```

<!DOCTYPE html>
<html lang="en">
  <head>
  :
	:
	:

    <script src="js/script.js"></script>
  </body>
</html>

```

Finally we can close the collapsed menu!!

![](http://i.imgur.com/T2pOUFY.gif)

