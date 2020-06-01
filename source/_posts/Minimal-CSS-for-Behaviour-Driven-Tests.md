---
title: Minimal CSS for Behaviour-Driven Tests
date: 2020-05-25 12:39:15
tags:
  - express
  - jasmine
  - zombie
  - testing
  - css
---

My buddy Dawson came back with some awesome questions a couple of weeks ago. He is brand new to writing software for the web and interested in learning a test-driven approach. He started [a new project](https://github.com/davyLSDev/transposer) but got hung up on writing some basic interface tests on his landing page. Upon closer inspection, it became clear he needed some direction in learning about [CSS selectors](https://developer.mozilla.org/en-US/docs/Learn/CSS/Building_blocks/Selectors).

<!-- more -->

To get the most use of this tutorial, read [Part 1](/2020/04/20/How-to-get-started-testing-with-Express-Jasmine-and-Zombie/) and [Part 2](/2020/05/04/How-to-get-started-testing-with-Express-Jasmine-and-Zombie-PART-DEUX/) of how to get set up testing with Jasmine and Zombie.

## transposer

According to Dawson, `transposer` is to transpose chords from a song to whichever key is desired. This is a great introductory project for him, because he's already implemented a non-web application that solves the same problem.

Dawson's first step is to build a cool landing page. Toward that end he added a new test to `spec/indexSpec.js`. As defined, when the application is loaded in the browser, we expect the `transposer` landing page to display the `transposer` logo:

```
it('displays the transposer icon', () => {
  // Hmmmmmmmm... how can this be done?
});
```

First, he tried this:

```
it('displays the transposer icon', () => {
  browser.assert.element("img:not(.has-error)");
});
```

This test fails, because he selected all images that don't have the class atribute `has-error`. This is the selector: `img:not(.has-error)`.

When I first cloned the repo, there were no images on the index page at all. As such, the test fails like this:

```
AssertionError [ERR_ASSERTION]: Expected 1 elements matching "img:not(.has-error)", found 0
```

Next, Dawson tried this:

```
it('displays the transposer icon', () => {
  browser.assert.element('#icon'.require);
});
```

This tests passes, but for all the wrong reasons. `'#icon'.require` is not really a valid CSS selector, nor is it good Javascript.  Here he is trying to dereference a property called `require` on a Javascript `string` type. A Javscript `string` has no property named `require`, so the command `'#icon'.require` returns `undefined`. He passed `undefined` to the Zombie `assert.element` function, which has no effect. No test was executed so it looks like it passed. This is really bad news and an important illustration of why tests need to fail before they pass.

Dawson is trying to test for the presence of a logo. When testing your app in a browser, you need to know how to address the elements being rendered to the screen. This includes the logo, which is rendered via an `img` element. [CSS selectors](https://developer.mozilla.org/en-US/docs/Learn/CSS/Building_blocks/Selectors) provide the means to do this. There is much to learn about the topic, but for this purpose it all boils down to _specifity_. With CSS selectors, you can retrieve any HTML element on your web page, but it's up to you to make sure you're retrieving the right ones.

Consider the following:

```
it('displays the transposer icon', () => {
  browser.assert.element('img');
});
```

This test ensures there's one image on your page (yes, the `img` selector is named for the `<img>` HTML element). Element names are not specific at all. They are the _least specific_, in fact. Asking for `img` by itself selects _every_ image currently rendered on the page. The assertion provided by Zombie only expects one image. Your app will likely have many images, so we need to be specific.

When you define an element in your HTML document, you may assign an `id` attribute (`<img id='icon' src='...'`>). An ID attribute is the _most specific_ selector all. There should only ever be one element on your page assigned a particular ID. This is kind of like how only one car should have a particular license plate. Unlike cars, however, there's nothing to stop you from abusing the expectation of unique IDs on a webpage. Do this at your own peril.

Knowing this, you might just want to apply unique IDs to all your HTML elements and write all your tests like this:


```
it('displays the transposer icon', () => {
  browser.assert.element('#icon');
});
```

This test isn't _wrong_ - it will pass - but it doesn't really describe what you're testing to the reader. I think the following is more descriptive and self explanatory (i.e., I'm selecting an image with ID attribute `icon`):

```
it('displays the transposer icon', () => {
  browser.assert.element('img#icon');
});
```

HTML and CSS are deceptively simple. I doubt anyone could convince everyone as to the _correct way_ to structure a document and apply styles. I generally favour establishing specificity through the nesting of [semantic tags](https://www.freecodecamp.org/news/semantic-html5-elements/) over managing unique CSS IDs and classes (class names are the _second_ most specific selector, FYI). Not only will this change your approach to testing, it will likely lead to a well-structured and easily understood document.

In Dawson's case, he's building a landing page for his app. He may decide to organize his logo or `#icon` in a page `header`, for example. I don't presume to suggest this is the _only_ way, but hopefully it invites consideration of how to best structure your documents so your CSS doesn't get too hairy:

```
it('displays the transposer icon', () => {
  browser.assert.element('header img[src="/images/transposer.png"]');
});
```

In any case, you won't know _the best way_ until you've tried a million different _wrong ways_. How would you lay out your `index.ejs` to make the above test pass?

If you learned something, support my work through [Wycliffe](https://www.wycliffe.ca/projects/dan-bidulock/) or [Patreon](https://patreon.com/whatdandoes).

