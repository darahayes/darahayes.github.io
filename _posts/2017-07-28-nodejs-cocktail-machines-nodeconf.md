---
title: "Node.js Cocktail Machines"
layout: post
date: 28-07-2017
tag:
- nodejs
- javascript
- hardware
- iot
- nodeconf
- mqtt
<!-- star: true -->
category: blog
author: darahayes
description: How we built Node.js cocktail machines for Nodeconf EU
photos:
  - url: /assets/images/nodeconf-2015-cocktail-machines-1.jpeg
    alt: Cocktail Machine Button
  - url: /assets/images/nodeconf-2015-cocktail-machines-2.jpeg
    alt: Cocktail Machine Button
  - url: /assets/images/nodeconf-2015-cocktail-machines-3.jpeg
    alt: Cocktail Machine Button
  - url: /assets/images/nodeconf-2015-cocktail-machines-5.jpeg
    alt: Cocktail Machine Button
  - url: /assets/images/nodeconf-2015-cocktail-machines-7.jpeg
    alt: Cocktail Machine Button
  - url: /assets/images/nodeconf-2015-cocktail-machines-8.jpeg
    alt: Cocktail Machine Button
  - url: /assets/images/nodeconf-2015-cocktail-machines-9.jpeg
    alt: Cocktail Machine Button
---

{% include slider-script.html %}

{% include image.html
  url="/assets/images/cocktail-machines-banner.jpeg"
  alt="Node.js Cocktail Machines"
  caption="The Finished Product"
  banner=true %}

For NodeConfEU 2015, we built and demoed Node.js powered cocktail machines! This was the last thing I did during my internship at nearForm.

We made four machines, each one powered by a raspberry pi with pumps for pouring drinks. All of the machines connected back to a server running on the local Wifi network, allowing conference attendees to order drinks using a browser.

{% include youtube.html 
url="https://www.youtube.com/embed/hKHZ1rSdAtI"
caption="Early Stage Testing." %}

In a week, we built the machines, wrote the software and perfected the recipes. (manual testing was required!). It was a pretty intense project. We were still crushing bugs and putting out fires minutes before the demo. Even during the demo we were pushing out software updates without interruption to the service!

In the end, over 100 drinks were served in an hour, and the project was a huge success! For the first time, I delivered and deployed software to a live audience. All the code can be found at our Github page liquid-io. Check out the videos and photos below.

{% include youtube.html 
    url="https://youtube.com/embed/cDsIYBXfHy0"
    caption="The Finished Product" %}

{% include slider.html
    photos=page.photos %} 