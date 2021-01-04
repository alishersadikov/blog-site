---
template: blog-post
title: "Ruby/Rails and Other Frameworks: Speed & Performance"
slug: /ruby-rails-performance
date: 2017-03-01 18:05
description: ruby rails performance
featuredImage: /assets/ruby-rails.jpeg
---
Having talked to a few developers who use Java or Go etc., I got this impression that most of them think of Ruby/Rails as an outdated and slow option that fewer and fewer companies are using. So this made me research this topic and I came up with a few answers.

To start off, as a developer, I am accustomed to the fact that I will have to learn a new language/framework every now and then and that it is counter-productive to limit myself to a few languages, let alone just one.

Why do we need our apps to be fast at all? Well, nobody argues that our customers would be happier if they do not have to wait for their transaction to complete on our site and happier customers ultimately lead to higher sales. So the research shows that such languages/frameworks as C++, Java, Scala, Go, Clojure, Node.js, etc. tend to be faster than Rails and a lot of Ruby developers and companies are switching to these or the other ones.

Although other factors such as reliability are also extremely important when comparing different frameworks, I just want to limit this discussion to speed. Basically, the question I want to answer is: can we adjust our Rails toolbelt so that it gets faster? Or can we use another Ruby framework to achieve the the speed comparable to faster frameworks.

I am going to reference the results of [Brian Knapp’s benchmark report](https://github.com/madebymarket/ruby-web-benchmark). He tested the combinations of: first, most Ruby frameworks starting form Rails and ending in microframeworks like Cuba. Second, runtime libraries such as most Ruby versions as well as jRuby. And third, web servers ranging form Webrick to Torqbox. He also relied on a basic “Hello World” app and an Apache Bench — processing 5000 requests as fast as possible, as well as Wrk for sending as many requests as possible over a 10 second period.

The results of this research are pretty amazing. Apparently, the fastest combination is jRuby(runtime) with Rack(framework) with Torqbox(server). The speed of 10,159 req/sec was achieved with this combination, which can be compared to 10,500 req/sec using Go.

Although this is a very limited test, but it gives us a clear idea that depending on the specs of our application and its usage, we can achieve great speeds by using the right combination of tools in our toolbelt.