---
title: 'Tut. 1: The Landing Page - Simple Photo Blogging'
tags:
- tutorial
- express
- testing
- jasmine
- zombie
---

You've got to start testing somewhere. Why not start by making sure the app actually loads successfully in the browser?

If you haven't already, go back and read [the introduction to this tutorial series](/2020/06/01/Simple-Photo-Blogging-for-Partnership-Development/). Make sure you've arrived for work on the `develop` branch as prescribed there.

The `photo-blog` application is nothing more than boiler plate code at this point. It was bootstrapped with `express-generator` and uses Embedded Javascript as its view engine. As far as modern web applications go, this is about as basic as it gets. 

This project pairs `jasmine` and `zombie` for testing. The former is a test framework while the latter is a _headless browser_. This means `zombie` has all the capabilities of a regular web browser, but doesn't actually render a web page to the screen. While this makes it a poor choice for browsing the web, it's a great tool for testing.

## The User Story







