---
# Common-Defined params

title: "How to Use Custom CSS on Hugo Site (With Pictures)"
date: "2022-09-20"
description: "How to Use Custom CSS on Hugo Site (With Pictures)"
categories:
  - "Tutorials"
tags:
  - "Technology"
  - "Tutorial"
  - "How-To"
  - "Hugo"
  - "Github"
  - "VS Code"
  - "Cloudflare Pages"
  - "JamStack"
  - "Visual Studio Code"

# menu: main # Optional, add page to a menu. Options: main, side, footer

# Theme-Defined params

# thumbnail: "img/placeholder.png" # Thumbnail image

# lead: "Example lead - highlighted near the title" # Lead text

comments: false # Enable Disqus comments for specific page
authorbox: true # Enable authorbox for specific page
pager: true # Enable pager navigation (prev/next) for specific page
toc: false # Enable Table of Contents for specific page
mathjax: false # Enable MathJax for specific page
sidebar: "right" # Enable sidebar (on the right side) per page
widgets: # Enable sidebar widgets in given order per page
  - "search"
  - "recent"
  - "taglist"
---

After setting up your Hugo site for the first time, next thing to do is to customize it to your own liking. One of the more common things you have to do is to change some of the styling (CSS).

**Here's how to do it.**

**Note:** I use Visual Studio code (VS Code). My IDE of choice. All steps below were executed from VSC. Should be doable as well from a different IDE.

## Step by Step Guide (With Pictures)

**1. Create a custom.css file. Place it in static/css folder inside your project folder.**

![](/img/Pasted_image_20220918231914.png)

**2. Open the custom.css file and type in all your styling changes.**

In the screenshot below, you can see that I wanted to put a border and box shadow on all image elements inside a paragraph (p tag).

![](/img/Pasted_image_20220918232205.png)

**3. Success!! Images in the post are now enclosed by a border and given a shadow as I wanted.**

Styling changes applied. See sample below (yellow highlighted).

![](/img/Pasted_image_20220918232556.png)

**4. Commit your code changes to your local repository and synch up to Github.**

Add a comment and click on Commit then Sync it to Github (remote repository)

![](/img/Pasted_image_20220918234508.png)

## That's pretty much it!

Let me know if you have questions.
