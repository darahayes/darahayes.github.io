---
title: "A Comprehensive Guide to Publishing Golang Libraries"
layout: post
date: 07-02-2018
tag:
- Golang
- Go
- Open Source
- Software
category: blog
author: darahayes
description: This post will give you all of the tools and tips you need to publish a high quality Golang library.
---

I decided I would learn a new programming language in 2018. So, for the past month I've been learning Go and the experience has been very enjoyable! I recently wrote my first Golang library. It's called [boom](https://github.com/darahayes/go-boom) and it makes HTTP error generation and handling very simple. 

I learned a few lessons along the way, so if you're like me and you're interested in publishing your first, or your second, or even your tenth library written in Go, this post is for you. This is a **comprehensive guide to publishing Golang libraries.**

## How do I publish a Library Written in Go?

Is your library publicly available on Github? If the answer is yes, then you've already published it. Now it's as simple as

```bash
go get https://github.com/username/library
```

and import the library in your code:

```golang
import (
  "github.com/username/library"
)
```

*Great, just push it to Github and job done! Why am I still reading this post?*

**But wait, there's more!** Dumping your code on Github will work, but if you hope someone will actually use it, you need to go the extra mile. I've compiled a checklist (mostly from other docs and blog posts) to ensure you write a high quality and attractive library. The best part? All of this stuff is super simple! 

In fact, you probably know this stuff already, but you just want to know some things specific to Golang.

## Step 1: Dependency Management

As a Node.js developer, this is something that tripped me up when I started learning Go. I was sorely missing something like [npm](https://www.npmjs.com/). Although you can use `go get` to fetch your dependencies, a package manager will scale much better in the long run.

I'm going to recommend [dep](https://github.com/golang/dep) because it's being developed by the Golang team. According to the Readme:

> dep is the *official* experiment, but not yet the official tool.

Initializing dep in a project is really simple:

```bash
dep init
# creates Gopkg.toml Gopkg.lock vendor/
```

The init command also works in existing projects. It does a good job of adding the dependencies based off your import statements. You can manually add dependencies too.

```bash
dep ensure -add github.com/foo/bar github.com/baz/quux
```

Check out the [getting started guide](https://golang.github.io/dep/docs/introduction.html) to learn more. There are also plenty of other similar tools such as [glide](https://github.com/Masterminds/glide) or [govendor](https://github.com/kardianos/govendor) which you can try too.

## Step 2: Tests

This goes without saying. One of the first things any developer will notice when they land on your library is whether or not you've tested your code. This is especially obvious in Go projects because the common convention is to have test files and source files in the same folder. For example, `foo.go` will have a `foo_test.go` in the same folder.

**Tests before tweets!** You only get once chance to do this right. Make sure you write tests before you start publicizing your code. Check out this wonderful article on [the basics of writing unit tests in Golang](https://blog.alexellis.io/golang-writing-unit-tests/).

## Step 3: Continuous Integration (CI)

Once you have some tests written, it's really important to use a CI service. At the very minimum this ensures new commits and pull requests against your project are tested. It's completely free for Open Source projects so there's no excuse. I personally love [CircleCI](https://circleci.com) but there are plenty of others such as [Travis](https://travis-ci.org/) and [Codeship](https://codeship.com).

Adding a project to CircleCI is simple. First sign up using your Github account, then follow the wizard to add your project. Lastly, create a `circle.yml` file in the root of your project and add the following:

```yaml
test:
  override:
    - go test
```

Technically you don't even need the `circle.yml` file. Most CI services will recognize that your project is written in Go and will automatically run the tests. I still recommend adding the file because **it makes it very clear to other developers** that you are using a CI service.


## Step 4: Code Coverage and coveralls.io

Good code coverage does not necessarily mean good quality code, but it's still a decent indicator. A developer is more likely to use your library if it has good code coverage. Golang has coverage reporting built in (awesome!), so you should take advantage of it.

```bash
go test -cover -coverprofile=coverage.out
```

You now have a code coverage report compatible with [coveralls.io](https://coveralls.io). Coveralls is an online service for code coverage reporting. It's free for Open Source projects and you can sign up using your Github account. Once you've signed up you can add your project.

I use a very neat tool called [goveralls](https://github.com/mattn/goveralls) to push coverage reports as part of my CircleCI builds. This is what the `circle.yml` file would look like:

```yaml
test:
  pre:
    - go get github.com/mattn/goveralls
  override:
    - go test -v -cover -race -coverprofile=/home/ubuntu/coverage.out
  post:
    - /home/ubuntu/.go_workspace/bin/goveralls -coverprofile=/home/ubuntu/coverage.out -service=circle-ci -repotoken=$COVERALLS_TOKEN
```

Now the build process is as follows: 

1. Install the goveralls tool.
2. Run the tests and also generate a coverage report.
3. Push the coverage report to Coveralls.

**Note** the $COVERALLS_TOKEN environment variable. This token can be found in the repo settings in coveralls.io and can be added as an environment variable in the CircleCI settings.

You should now see reports in Coveralls after every build.

{% include image.html
url="/assets/images/coveralls.png"
alt="Project in coveralls.io"
caption="A Project in coveralls.io" %}

## Step 5: Documentation and godoc.org

At the very minimum, your library should have a Readme file describes your project. Some things you should include:

* High level description and the problem your library solves.
* Explain why someone would use your library.
* Code snippets demonstrating common usage.

Go also provides tooling to generate API docs from comments in the your code. Below is a very basic example:

```golang
  // BadRequest responds with a 400 Bad Request error.
  // Takes an optional message of either type string or type error,
  // which will be returned in the response body.
  func BadRequest(w http.ResponseWriter, message ...interface{}) {
    // code in here
  }
```

You can use the `godoc` tool (installed with Go) to build docs.

```bash
godoc -http=":6060"
```

This runs a local webserver where you can browse the documentation for all packages on your machine. You can visit the docs for your package at the  `/pkg/github.com/<username>/<library>` path.

Here's a screenshot of the documentation for the above function:

{% include image.html
  url="/assets/images/go-doc-example.png"
  alt="Example of Golang API docs"
  caption="Example of Golang API docs" %}

Once you push your library to Github, you can access the same docs online at `godoc.org/github.com/<usernme>/<library>`. You should **link the godoc.org documentation in your readme.**

This example is trivial but the docs generation is powerful and has some impressive tricks up its sleeve. I recommend you look at this official Golang blog post on [Documenting Go Code](https://blog.golang.org/godoc-documenting-go-code) and this amazing [tutorial](https://godoc.org/github.com/fluhus/godoc-tricks) for more info.

## Step 6: Go Report Card

[goreportcard.com](https://goreportcard.com) is a free service that analyzes Golang projects. Simply enter your project's Github link to receive a detailed report and an overall score. The service checks for a number of issues such as formatting and lint errors, [cyclomatic complexity](https://en.wikipedia.org/wiki/Cyclomatic_complexity) issues, spelling issues and licensing issues.

The tool provides an impressive level of detail and also suggests ways to fix the problems.

{% include image.html
  url="/assets/images/go-report-card.png"
  alt="Go Report Card Example"
  caption="Go Report Card Example" %}

## Step 7: License

This is arguably the most important step, yet so many people forget about it. **Your code is not open source if it does not have a license.** Even if the code is available. Without a license, developers and businesses cannot safely use your code. It is up to you to choose the appropriate license. For small libraries I stick to the MIT license as it gives the most freedom. If you're not sure you can try [choosealicense.com](https://choosealicense.com/).

I recommend you add a file called `License` to your repo. [opensource.org](https://opensource.org/licenses) has links to licenses you can copy and paste.

## Step 8: Readme Badges

You can tie all of the previous steps together and give your Readme some fancy badges.

{% include image.html
  url="/assets/images/repo-badges.png"
  alt="Status Badges in Readme"
  caption="Status Badges in Readme.md" %}

You might dismiss this as being unimportant but **first impressions are everything**. These badges tell incoming developers that you put great care into your work. 

The build status and code coverage badges can be found in the CircleCI and Coveralls settings pages. The Godoc and the Go Report badges can be added with the following markdown:

```
[![Documentation](https://godoc.org/github.com/<username>/<library>?status.svg)](http://godoc.org/github.com/<username>/<library>)
[![Go Report Card](https://goreportcard.com/badge/github.com/<username>/<library>)](https://goreportcard.com/report/github.com/<username>/<library>)
```

Just make sure you modify the links to match your project in Github.

## Step 9: Draft a Release

The last thing I recommend is to [draft a release in Github](https://help.github.com/articles/creating-releases/). This allows other developers to depend on a specific version of your code. A lot of developers are using package managers such as [dep](https://github.com/golang/dep) or [glide](https://github.com/Masterminds/glide) which rely on git tags for versioning. If you are not aware of Semantic Versioning, definitely check out [semver.org](https://semver.org/).

## Wrapping up

So at this stage you have your own library. It's really well tested and documented. It's available on Github and it has an Open Source license. If you've made it this far, that's great! These steps above should be enough to deliver a high quality library. However, there are further steps you can take, especially if you want your library to thrive in the world of Open Source.

* Add a contribution guide.
* Add a code of conduct.
* Create a Gitter/Slack/IRC channel where users and potential contributors can get in touch.
* Share your project - Twitter, Blog Posts, Reddit, StackOverflow.
* Create a homepage for your project.

[opensource.guide](https://opensource.guide) is a very useful website to learn more about building successful Open Source projects.

I hope you found this post useful. If you feel like I've missed anything please let me know in the comments. Thanks for reading!


