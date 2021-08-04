---
layout: default
title: Personal Blog
description: Articles about programming and a place to keep my own notes and recipes.
tags: Jekyll HTML CSS JavaScript NodeJS Express DigitalOcean Linux VPS
---

# My Personal Blog (this Website)

![text](/assets/images/projects/blog/personal-website-screenshot.png)

Originally I wanted something more convenient than the notes app on my phone for keeping track of different recipes I have learned. Rather than scrolling through pages of small text while trying not to burn anything, I figured I could just make a simple blog that would be easier to navigate on my phone.

I have also added various posts about different programming topics, both as a review for my own learning and as a reference for later, though if anyone found any of them helpful that would be great too.

## Design

The blog itself is fairly simple. It is built with HTML and Jekyll. As a static site generator Jekyll does pretty well, with most things being easily accomplished (like being able to write posts in a markdown format).

Aside from that I got to experiment a lot with the Sass to try and find a style I liked for reading on my phone as well as on a laptop or desktop, and I got some practice customizing different themes as well, though I ultimately ended up building my own from scratch for this.

The website itself is deployed to DigitalOcean on a Linux VPS. I have Nginx running on the VPS as a proxy, which lets me very easily hook up both free SSL for HTTPS and my domain name using the Certbot tool. The app itself is kept running in the background and Nginx manages requests between it and the outside, and since it is a static site it is very stable and inexpensive. 

## Downloading the Code

If you would like to explore this more, then check out the code on [GitHub](https://github.com/JamesQuillin/personal-website).

If the deployment setup sounds interesting to you then check out [this post](/articles/simple-website-deployment) I wrote detailing the whole process. It's pretty simple and a very nice way to deploy almost any personal project (or even a tutorial project if you are a beginner). It gives you a lot more control than you get from a service that handles most of the deployment for you and can be a great learning experience too.
