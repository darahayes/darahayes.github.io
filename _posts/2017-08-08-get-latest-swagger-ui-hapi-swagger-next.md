---
title: "Get the Latest Swagger UI with hapi-swagger-next"
layout: post
date: 07-08-2017
tag:
- nodejs
- hapi
- swagger
- documentation
category: blog
author: darahayes
description: 
---

> A fork of the excellent hapi-swagger module that delivers the latest version of the Swagger UI

The [hapi-swagger](https://npmjs.com/package/hapi-swagger) module is a seriously high quality project. However, activity has slowed a little. At the time of writing (August 2017) It serves version `2.2.4` of the Swagger UI which is quite outdated compared to the more current `3.x` versions. It looks and feels dated. I decided to fork hapi-swagger and publish [hapi-swagger-next](https://npmjs.com/package/hapi-swagger-next). It's a drop-in replacement for hapi-swagger with no changes needed.

If you're not familiar with hapi-swagger, you should check out my article that shows [how to auto generate swagger documentation for your hapi server](/document-your-hapi-application-swagger/).

### Usage

It's pretty simple

```
npm install hapi-swagger-next
```

and change

```js
require('hapi-swagger')
```

to

```js
require('hapi-swagger-next')
```

Here's a screenshot of what it looks like on an example Users API I've written:


{% include image.html 
url="/assets/images/hapi-swagger-next-users-api.png"
alt="The Latest Swagger UI using hapi-swagger-next"
caption="The Latest Swagger UI using hapi-swagger-next" %}

Right off the bat, this is a lott prettier looking than the old UI. Here's a detailed view of an endpoint on my example math API: (See full code below)

{% include image.html 
url="/assets/images/hapi-swagger-next-detail.png"
alt="Detailed View of the latest Swagger UI"
caption="Detailed View of the latest Swagger UI" %}

Just like the older Swagger UI, you can see the required inputs and their data types. You can see what an example response looks like, and you can test the endpoint in your browser. Except it's prettier!

### Notes

Ideally, I would like the original hapi-swagger module to serve the latest UI as opposed to having a fork. I've [created an issue](https://github.com/glennjones/hapi-swagger/issues/443) on the original project addressing this. Hopefully we can eventually get this functionality merged back into the original project.

### Try it out

If you want to try it out real quick, you can use the example API below.

{% gist darahayes/083dc2fa07218d1eac182080eaa1a5cc %}