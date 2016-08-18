---
layout: post
title:  "Hello World - Time for a Makeover"
date:   2016-08-07 11:00:00 -0500
# categories: update
comments: false
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

[personal-website-v1]: ../../../../assets/personalwebsitepic.png "My old personal website"
[1]: https://www.spotify.com/us/
[2]: https://www.airbnb.com/
