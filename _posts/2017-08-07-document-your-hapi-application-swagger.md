---
title: "Auto Document your Hapi Application with Swagger"
layout: post
date: 07-08-2017
tag:
- nodejs
- hapi
- swagger
- documentation
category: blog
author: darahayes
description: Documenting APIs can be tricky and time consuming. Today I will show you how to keep your Hapi application's docs lean and accurate with hapi-swagger.
---

Over the past two years I've worked on many applications built with [Hapi.js](https://npmjs.com/package/hapi). It's been our main choice for building APIs at [nearForm](https://www.nearform.com) for some time. 

Documenting APIs can be tricky and time consuming. In many cases, we don't have enough manpower to keep docs accurate throughout a project. That being said we do need __some__ documentation. Enough to keep us productive as a team.

Today I will show you how to keep your Hapi application's docs __lean__ but fairly __accurate__ with the help of [hapi-swagger](https://npmjs.com/package/hapi-swagger). 


If you want you can scroll to the very bottom for the full code.

### Swagger

You may have heard of [Swagger](https://swagger.io) already, but what is it? Basically, it's a bunch of tooling to help you clearly define your API in a standardized way. This makes it really easy to document among other things. Check out [the website](https://swagger.io) for more detail.

### Swagger Specification

A Swagger Specification is a JSON/YAML schema that describes your RESTful API and its operations, its inputs and outputs, its constaints etc.

### Swagger UI

The Swagger UI takes a Swagger Specification and renders it into a set of interactive docs. Check out Swagger's [live demo](http://petstore.swagger.io/). The demo describes the API endpoints and shows example inputs/outputs, required fields, etc. You can even test the actual endpoints from the UI itself. Pretty Neat!

### Auto Generate Swagger Docs for your Hapi API

Hapi is very config oriented and the code is super declarative. It turns out it's relatively easy to auto-generate Swagger Specifications for Hapi applications.

Take the following Hapi Server:

```js
const Hapi = require('hapi')
const Joi = require('joi')
const Boom = require('boom')

const server = new Hapi.Server()
server.connection({ host: 'localhost', port: 8000 })

const operations = {
  'add': (a, b) => {
    return a + b
  },
  'subtract': (a, b) => {
    return a - b
  },
  'multiply': (a, b) => {
    return a * b
  },
  'divide': (a, b) => {
    if (b === 0) return Boom.badRequest('cannot divide by 0')
    return a / b
  }
}

server.route({
  method: 'GET',
  path:'/math', 
  handler: (request, reply) => {
    let {a, b, op} = request.query
    let operation = operations[op]
    let response = {
      values: {
        'a': a,
        'b': b
      },
      operation: op,
      result: operation(a, b)
    }
    return reply(response)
  },
  config: {
    validate: {
      query: {
        a: Joi.number().required(),
        b: Joi.number().required(),
        op: Joi.string().valid(Object.keys(operations)).required()
      }
    }
  }
})

server.start((err) => {
  if (err) throw err
  console.log('Server running at:', server.info.uri)
})
```

This server has a single `/math` endpoint that takes the numbers `a` and `b`, and `op` which is one of `['add', 'subtract', 'multiply', 'divide']`. We can make a request like so:

```
curl 'http://localhost:8000/math?op=multiply&a=5&b=50'

{
  "values": {
    "a": 5,
    "b": 50
  },
  "operation": "multiply",
  "result": 250
}
```

We can serve a Swagger UI from our app with the [hapi-swagger](https://npmjs.com/package/hapi-swagger) module:

```js
server.register(
  [
    {
      register: require('inert')
    },
    {
      register: require('vision')
    },
    {
      register: require('hapi-swagger'),
      options: {
        info: {
          title: 'Math API',
          description: 'The Math API lets you perform basic arithmetic operations over HTTP',
          version: '1.0.0'
        }
      }
    }
  ]
)
```

In the options object for `hapi-swagger` we have added some basic information about the API such as the title, version and description. As well as `hapi-swagger` we are registering [inert](https://npmjs.com/package/inert) and [vision](https://npmjs.com/package/vision) which are required by hapi-swagger.

Now we can add some small decoration to our endpoint as follows:

```
config: {
  validate: {
    query: {
      a: Joi.number().required(),
      b: Joi.number().required(),
      op: Joi.string().valid(Object.keys(operations)).required()
    }
  },
  tags: ['api'],
  description: 'Takes in two numbers (a,b) and performs the desired operation on them'
}
```

The `tags` and `description` are used to document our endpoint. At the minimum `tags: ['api']` is needed for the endpoint to be shown in the Swagger docs. Now you can look at the docs at [localhost:8000/documentation](http://localhost:8000/documentation)

{% include image.html 
  url="/assets/images/swagger-docs-overview.png"
  alt="Auto Generated Swagger Documentation"
  caption="Auto Generated Swagger Documentation" %}

{% include image.html 
url="/assets/images/swagger-docs-detailed.png"
alt="Viewing Endpoints in the Swagger UI"
caption="Detailed Endpoint Info in Swagger UI" %}

You can even test endpoints directly from the UI:

{% include image.html 
url="/assets/images/swagger-docs-test.png"
alt="Testing Endpoints in the Swagger UI"
caption="Testing Endpoints in the Swagger UI" %}

#### Document Response Objects

It's also possible to document what a response from an endpoint will look like. Using `joi` we can define a schema like so:

```js
const mathResponse = Joi.object({
  values: {
    a: Joi.number(),
    b: Joi.number()
  },
  operation: Joi.string().valid(Object.keys(operations)).required(),
  result: Joi.number()
})
```

and then the endpoint config becomes:

```
config: {
  validate: {
    query: {
      a: Joi.number().required(),
      b: Joi.number().required(),
      op: Joi.string().valid(Object.keys(operations)).required()
    }
  },
  tags: ['api'],
  description: 'Takes in two numbers (a,b) and performs the desired operation on them',
  response: {schema: mathResponse}
}
```

Now the Swagger UI becomes even more helpful:

{% include image.html 
url="/assets/images/swagger-docs-response.png"
alt="Swagger Docs Showing Example Response"
caption="Swagger Docs Showing Example Response" %}

### Conclusion

So there you have it! By adding a tiny amount of metadata to our Hapi routes, we can get auto generated docs! It's not perfect, but it's pretty decent. Because these docs are generated from the code itself, they tend to stay fairly accurate and up to date. They're minimalistic and don't provide very deep insight but they're more than enough for things like developer onboarding.

### Full code

You can view the full code in this [gist.](https://gist.github.com/darahayes/70f951e12aa74bd95b2058ee4141a319)

{% gist darahayes/70f951e12aa74bd95b2058ee4141a319 %}