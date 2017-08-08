---
title: "Talk: Self Detect a Memory Leak in Node.js"
layout: post
date: 01-07-2017
tag:
- talks
- v8
- nodejs
- javascript
- memory
<!-- star: true -->
category: blog
author: darahayes
description: Dara Hayes Speaks about how to detect a memory leak in node.js applications using tools like heapdump, memwatch and node's --inspect flag
image: /assets/images/darahayes-logo.png
---

{% include youtube.html 
url="https://www.youtube.com/embed/NVyMX3HMyrs"
caption="Detecting Memory Leaks by Dara Hayes" %}

Back in June 2017, I was so happy to deliver my first ever tech talk about 'Detecting Memory Leaks in Node.js.' The talk was given at the [Node Dublin Meetup](https://www.meetup.com/Dublin-Node-js-Meetup/) at the Intercom offices.

I got the idea for the talk after updating [this nearForm blogpost](https://www.nearform.com/blog/self-detect-memory-leak-node/). In this talk, I give a high level overview of memory management and garbage collection in V8 - The Node.js runtime. Then I jump into a practical demo to show how memory leaks can be found using a number of tools such as [heapdump](http://npm.im/heapdump), [memwatch](http://npm.im/memwatch-next), and Node's `--inspect` flag.

I'm pretty happy with how my first speaking engagement went. I have a lot to learn but I really look forward to doing more talks. Thanks to [nearForm](https://nearform.com) who kindly sponsored the event and uploaded a video of the talk.

Take a look at the slides below. [link](https://slides.com/darahayes/memory-leaks-nodejs/)

{% include youtube.html 
url="//slides.com/darahayes/memory-leaks-nodejs/embed?style=light"
caption="Detecting Memory Leaks by Dara Hayes" %}