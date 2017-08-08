---
title: "Talk: Minishift as a Development Environment for Node.js"
layout: post
date: 01-08-2017
tag:
- talks
- nodejs
- minishift
- openshift
- Red Hat
category: blog
author: darahayes
description: Featured on the Openshift blog, Dara Hayes Speaks about one potential approach to using Minishift and OpenShift as a Development Environment for Node.js, including support for instant reload and stateful services.
image: /assets/images/darahayes-logo.png
---

{% include youtube.html 
url="https://www.youtube.com/embed/d5JNUsQ7k2Q"
caption="Minishift as a Development Environment for Node.js by Dara Hayes" %}

In July of 2017, I got the chance to do an Openshift Commons Briefing about 'Minishift as a Development Environment for Node.js.' which was [featured](https://blog.openshift.com/openshift-commons-briefing-84-upskilling-on-openshift-using-minishift-and-node-js-dara-hayes-nearform/) on the Openshift Blog.

Every week the [Openshift Commons](https://commons.openshift.org/) community invites somebody to speak about topics relating to [Red Hat OpenShift](https://openshift.io). It was a great honour to do this in collaboration with Red Hat for my second ever talk!

## Summary
In this briefing, I discuss how Minishift can be used as a local development environment for OpenShift and demonstrate one approach to developing Node.js applications with OpenShift/Minishift. I also provide a live demo of the workflow and discuss some of the material in the [minishift-demo](https://github.com/nearform/minishift-demo) repo.

A good local development workflow has quick feedback loops, meaning code changes are reflected instantly. OpenShift’s build and deploy process can be too slow in that context, so at nearForm we set out on a journey to optimize this process.

At nearForm, a team was assembled and tasked with evaluating Minishift as a local development environment for clients moving to OpenShift. Our goal was to create a local environment that mirrored production as much as possible and to upskill people on Openshift at the same time.

The end result was a Node.js app running in a local OpenShift cluster that could be live reloaded without rebuilding containers. Along the way, we learned about many of the core benefits of OpenShift such as the integrated build system, integrated docker registry, the powerful web console and templates.

Myself and Conor O’Neill also discussed challenges encountered and provided some useful tips to those starting with Node on OpenShift.

Take a look at the slides below. [link](http://slides.com/darahayes/minishift-as-a-dev-environment-node)

{% include youtube.html 
url="//slides.com/darahayes/minishift-as-a-dev-environment-node/embed?style=light"
caption="Minishift as a Development Environment for Node.js by Dara Hayes" %}