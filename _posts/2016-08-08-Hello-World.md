---
layout: post
title:  "Hello World - Time for a Makeover"
date:   2016-08-19 11:00:00 -0500
# categories: update
comments: true
author: Aaron Young
---

It's the year 2k16, and the last thing anyone wants to see on the internet is a
boring website: uncreative layouts, barely responsive and dynamic design, and
BASIC out-of-the-box Bootstrap elements. Unfortunately, that accurately
described my personal website...

![personal-website-v1]

Now that I have graduated from college, I have all this time on my hands, it is
time to redesign my own piece of real estate on the Web. It's also a great way
to practice my HTML/CSS/Javascripty/JQuery. Let's get started!

---

Initial Ideas
---

Here are some key elements I knew I wanted for my new website:

* Create something similar to [Spotify][1] or [Airbnb][2]
* Single page
* Responsive, mobile friendly
* Fixed-top navigation bar
* Full screen sections
* Smooth, animated scrolling
* Hosted on GitHub Pages

At this point, if you're thinking, "Isn't that just any run-of-the-mill website?".
You're absolutely right. The following tweet perfectly captures the frustration towards the
lack of diversity in modern websites today:

<blockquote class="twitter-tweet tw-align-center" data-lang="en"><p lang="en" dir="ltr">which one of the two possible websites are you currently designing? <a href="https://t.co/ZD0uRGTqqm">pic.twitter.com/ZD0uRGTqqm</a></p>&mdash; @jongold (@jongold) <a href="https://twitter.com/jongold/status/694591217523363840">February 2, 2016</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

Unfortunately, I will not be diversifying the world wide web today, but some day
I will. Everyone has to start somewhere though, amrite?!

---

Setting up my Development Environment
---
I simply organized my directory so that my HTML, CSS, JS, and other media files are in separate places like so:

```
.
+-- index.html
+-- .gitignore
+-- CNAME
+-- assets
|   +-- css
|   |   +-- index.css
|   |   +-- bootstrap.min.css
|   +-- js
|   |   +-- index.js
|   |   +-- bootstrap.min.js
|   |   +-- jquery-2.1.4.js
|   +-- images
```
---

Initial index.html
---

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <!-- The above 3 meta tags *must* come first in the head; any other head content must come *after* these tags -->

    <!-- Latest compiled and minified CSS -->
    <link rel="stylesheet" href="assets/css/index.css"/>
    <link rel="stylesheet" href="assets/css/bootstrap.min.css">

    <!-- Latest compiled JavaScript -->
    <script src="assets/js/index.js"></script>
    <script src="assets/js/bootstrap.min.js"></script>
    <script src="assets/js/jquery-2.1.4.js"></script>

    <title>Aaron Young</title>

  </head>
  <body>
    <div class="container-fluid">
    </div>
  </body>
</html>
```

We are now ready to start building our website!

Navigation Bar
---

No website can be browsed without a navigation bar. Let's start there. I want
to take advantage of Bootstrap's [Navigation Bar with smooth scrolling][3] and
[Collapsing Navigation Bar][4].

HTML
---

First, let's add ```data-spy```, ```data-target``` and ```data-offset``` attributes
to the scrollable area. In our case, this is the ```<body>```

```
<!-- Smooth scrolling -->
<body data-spy="scroll" data-target=".navbar" data-offset="50">
  <div class="container-fluid">
.
.
.
```

Next, we will add the fixed-top Navigation bar in our ```<div class="container-fluid">```,
which contains a header, and a collapsable navbar.

```
<!-- Smooth scrolling -->
<body data-spy="scroll" data-target=".navbar" data-offset="50">
  <div class="container-fluid">
    <div class="navbar navbar-default navbar-fixed-top">
      <div class="navbar-header">
        <button type="button" class="navbar-toggle" data-toggle="collapse" data-target="#myNavbar">
          <span class="icon-bar"></span>
          <span class="icon-bar"></span>
          <span class="icon-bar"></span>
        </button>
        <a class="navbar-brand" href="#home" class="pull-left">Aaron Young</a>
      </div>

      <!-- Collapsing Navigation Bar -->
      <div class="collapse navbar-collapse" id="myNavbar">
        <ul class="nav navbar-nav navbar-right" id="nav">
          <li class="active"><a href="#home">Home</a></li>
          <li><a href="#about">About</a></li>
          <li><a href="#education">Background</a></li>
          <li><a href="#work">Work</a></li>
          <li><a href="#projects">Projects</a></li>
          <li><a href="#contact">Contact</a></li>
        </ul>
      </div>
    </div>
  </div>
</body>
```

CSS
---
Now that we have a basic Bootstrap fixed-top collapsable navbar, let's style it up with a little CSS.

```
.container-fluid {
  padding: 0px !important;
}

/* navbar properties */
.navbar {
  background-color: rgba(0,0,0,0.5) !important;
  border: 0 !important;
  padding-top: 10px;
  visibility: visible;
}

/* navbar invisible */
.navbar-hide {
  visibility: hidden;
}

/* navbar nav space from right */
.navbar-nav {
  margin-right: 5vw;
}

/* navbar nav individual spacing */
.navbar-nav > li {
  margin-left: 1vw;
}

/* navbar active background properties */
.nav .active a {
  background: none !important;
  border-bottom: 5px solid #89C4F4;
}

/* navbar active text color */
.navbar-nav > .active> a, .navbar-nav> li >a:hover, .navbar-nav>.active>a:focus {
  color: #89C4F4 !important;
  border-bottom: 5px solid #89C4F4;
}

/* section properties for testing purposes */
.section {
  min-height: 105vh;
  padding: 60px;
}

/* navbar text properties */
.navbar .nav > li > a {
    color: white;
    font-size: 15px;
    font-weight: 100;
}

/* toggle triple-bar color */
.icon-bar {
  color: white;
}

/* remove the border around toggle button */
.navbar-toggle {
  border: none;
}

.navbar-default .navbar-toggle .icon-bar {
    background-color: white !important;
}

/* navbar brand properties */
.navbar .navbar-brand {
  color: white;
  font-size: 25px;
  padding-left: 30px;
  font-weight: 100;
}

/* navbar brand hover properties */
.navbar-brand:hover {
  color: #89C4F4 !important;
}

@media only screen and (max-width: 768px) {
    [class*="col-"] {
        width: 100%;
    }

    /* navbar active background properties */
    .nav .active a {
      border-bottom: none;
    }

    /* navbar active text color */
    .navbar-nav > .active> a, .navbar-nav> li >a:hover, .navbar-nav>.active>a:focus {
      border-bottom: none;
    }
}
```

Javascript
---
Last but not least, let's add animated scrolling with some javascript!

```javascript
$(document).ready(function () {
  $("#nav li a").on("click", navbarBtnClick);
  $(document).on("click", closeToggle);
});

/* close toggle if it's open and clicked outside toggle */
function closeToggle(event) {
  var clickover = $(event.target);
  var _opened = $(".navbar-collapse").hasClass("in");
  if (_opened === true && !clickover.hasClass("navbar-toggle")) {
      $("button.navbar-toggle").click();
  }
}

function navbarBtnClick(event) {
  // Make sure this.hash has a value before overriding default behavior
  if (this.hash !== "") {

    // Prevent default anchor click behavior
    event.preventDefault();

    // Store hash (#)
    var hash = this.hash;

    // Using jQuery's animate() method to add smooth page scroll
    // The optional number (800) specifies the number of milliseconds it takes to scroll to the specified area (the speed of the animation)
    $('html, body').animate({
      scrollTop: $(hash).offset().top
    }, 800, function(){

      // Add hash (#) to URL when done scrolling (default click behavior)
      window.location.hash = hash;
    });
  } // End if statement
  $(".navbar-collapse").collapse('hide');
}
```

Result
---
And there you have it! A nice looking fixed-top animated navigation bar.

<p data-height="443" data-theme-id="0" data-slug-hash="LkvGgm" data-default-tab="result" data-user="aaronyoung" data-embed-version="2" class="codepen">See the Pen <a href="http://codepen.io/aaronyoung/pen/LkvGgm/">Personal-Website-Navbar</a> by Aaron Young (<a href="http://codepen.io/aaronyoung">@aaronyoung</a>) on <a href="http://codepen.io">CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

Helpful links
---
* [w3school: Bootstrap Scrollspy][3]
* [w3school: Bootstrap Navbar][4]
* [Stackoverflow: Change color of bootstrap navbar on hover link?][6]
* [Stackoverflow: navbar active li not changing background-color][7]
* [Stackoverflow: Change icon-bar (☰) color in bootstrap][8]
* [Stackoverflow: How to close an open collapsed navbar when clicking outside of the navbar element in Bootstrap 3?][5]
* [How to Center Align Your Embedded Tweets][9]

[personal-website-v1]: ../../../../assets/personalwebsitepic.png "My old personal website"
[1]: https://www.spotify.com/us/
[2]: https://www.airbnb.com/
[3]: http://www.w3schools.com/bootstrap/bootstrap_scrollspy.asp
[4]: http://www.w3schools.com/bootstrap/bootstrap_navbar.asp
[5]: http://stackoverflow.com/questions/23764863/how-to-close-an-open-collapsed-navbar-when-clicking-outside-of-the-navbar-elemen
[6]: http://stackoverflow.com/questions/16625972/change-color-of-bootstrap-navbar-on-hover-link
[7]: http://stackoverflow.com/questions/20732584/bootstrap-3-navbar-active-li-not-changing-background-color
[8]: http://stackoverflow.com/questions/20540563/change-icon-bar-color-in-bootstrap
[9]: http://blog.hubspot.com/blog/tabid/6307/bid/34273/How-to-Center-Align-Your-Embedded-Tweets-Quick-Tip.aspx#sm.0001hboq1hv0aevvzvm123xijzokc
