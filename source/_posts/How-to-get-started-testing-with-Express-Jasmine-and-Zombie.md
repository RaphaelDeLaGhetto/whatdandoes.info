---
title: 'How to get started testing with Express, Jasmine, and Zombie'
tags:
  - express
  - jasmine
  - zombie
  - testing
date: 2020-04-20 21:34:41
---


Okay, so I'm a little old fashioned. I'm also lazy and reluctant to learn new things. My buddy Dawson asked _How do I get started with testing my web software?_ I said, start with [node/express](https://expressjs.com/)...

<!-- more -->

# Express

Express is a web framework. You build server-side software with Express. Let's bootstrap a project with [express-generator](https://expressjs.com/en/starter/generator.html):

```
npx express-generator --view=ejs myapp
```

This creates a skeleton application from which to launch development. The `ejs` stands for _Embedded Javascript_. To install:

```
cd myapp
npm install
```

Execute the server:

```
npm start
```

Assuming all is well, you can navigate to http://localhost:3000 to see your new app in action. Halt server execution by pressing _Ctrl-C_.

That's great and all, but let's get to testing...

# Jasmine

I'm not sure [Jasmine](https://jasmine.github.io/) is trendy, but I've been using it for years. It takes minimal setup and can be neatly structured. It's a good tool and the tests you write are easily adapted for execution by other test frameworks (if you decide you don't like `jasmine`).

Add `jasmine` to your web application:

```
npm install --save-dev jasmine
```

`jasmine` is now a development dependency. Execute `cat package.json` to get a peak at how `node` manages its dependencies.

Initialize `jasmine` like this:

```
npx jasmine init
```

Most people like to script test execution with `npm`. Open the `package.json` file just mentioned. Configure `"scripts": { "test": "jasmine" }`... that is, make `package.json` look like this:

```
{
  "name": "myapp",
  "version": "0.0.0",
  "private": true,
  "scripts": {
    "start": "node ./bin/www",
    "test": "jasmine"
  },
  "dependencies": {
    "cookie-parser": "~1.4.4",
    "debug": "~2.6.9",
    "ejs": "~2.6.1",
    "express": "~4.16.1",
    "http-errors": "~1.6.3",
    "morgan": "~1.9.1"
  },
  "devDependencies": {
    "jasmine": "^3.5.0"
  }
}
```

If you do this correctly, you can run the following:

```
npm test
```

And expect to see something like this:

```
> myapp@0.0.0 test /home/daniel/workspace/myapp
> jasmine

Randomized with seed 44076
Started


No specs found
Finished in 0.003 seconds
Incomplete: No specs found
Randomized with seed 44076 (jasmine --random=true --seed=44076)
npm ERR! Test failed.  See above for more details.
```

`jasmine` is telling us that the tests failed because we have yet to write any. Being as lazy as I am, I try to find opportunities to skip unit tests and go directly to testing user behaviour. For this, I use a _headless_ browser called [Zombie](http://zombie.js.org/).

# Zombie

Like `jasmine`, I'm not sure `zombie` is the _trendiest_ option out there, but I've always managed to get the two to play nicely together and have yet to find any serious shortcoming. Add `zombie` to your project like this:

```
npm install --save-dev zombie
```

Now we're ready to write some tests...

# Testing!

Oh wait, what's this app supposed to do? _Ummmmm..._

For now, I'll keep it really simple until my buddy Dawson comes up with tougher testing questions.

## Purpose

**I want to be able to load my app in a browser, enter my name in an input field, press _Submit_, and receive a friendly greeting in return.**

When starting a new web application, the first test I write ensures my page actually loads in the `zombie` browser. Create a _spec_ file:

```
touch spec/indexSpec.js
```

I use the word _index_ in the sense that it's the default first page you land on at any website. Test files end with the _*Spec.js_ suffix by default. Execute `cat spec/support/jasmine.json` to see how `jasmine` decides which files to execute.

Open `spec/indexSpec.js` in your favourite editor and paste this:

```
const Browser = require('zombie');

Browser.localhost('example.com', 3000);

describe('the landing page', () => {

  let browser;

  /**
   * This loads the running web application
   * with a new `zombie` browser before each test.
   */
  beforeEach(done => {
    browser = new Browser();

    browser.visit('/', err => {
      if (err) return done.fail(err);
      done();
    });
  });

  /**
   * Your first test!
   *
   * `zombie` has loaded and rendered the page
   * returned by your application. Use `jasmine`
   * and `zombie` to ensure it's doing what you
   * expect.
   *
   * In this case, I just want to make sure a 
   * page title is displayed.
   */
  it('displays the page title', () => {
    browser.assert.text('h1', 'Friendly Greeting Generator'); 
  });

  /**
   * Put future tests here...
   */
  
  // ...
});
```

Simple enough. At this point you might be tempted to go make the test pass. Instead, execute the following to make sure it fails:

```
npm test
```

Whoa! What happened? You probably see something like this:

```
Randomized with seed 73862
Started
F

Failures:
1) the landing page displays the page title
  Message:
    Failed: connect ECONNREFUSED 127.0.0.1:3000
  Stack:

    ...
```

It's good that it failed, because that's an important step, but if you look closely at the error, `connect ECONNREFUSED 127.0.0.1:3000` tells you your app isn't even running. You'll need to open another shell or process and execute:

```
npm start
```

Your app is now running and `zombie` can now send a request and expect to receive your landing page. In another shell (so that your app can keep running), execute the tests again:

```
npm test
```

If it fails (as expected), you will see something like this:

```
Randomized with seed 38606
Started
F

Failures:
1) the landing page displays the page title
  Message:
    AssertionError [ERR_ASSERTION]: 'Express' deepEqual 'The Friendly Greeting Generator'
  Stack:
    error properties: Object({ generatedMessage: true, code: 'ERR_ASSERTION', actual: 'Express', expected: 'Friendly Greeting Generator', operator: 'deepEqual' })
```

That's much better. Now, having ensured the test fails, make the test pass. Open `routes/index.js` in your project folder and make it look like this:

```
var express = require('express');
var router = express.Router();

/* GET home page. */
router.get('/', function(req, res, next) {
  // The old command
  //res.render('index', { title: 'Express' });

  // The new test-friendly command
  //res.render('index', { title: 'The Friendly Greeting Generator' });
});

module.exports = router;
```

Execute the tests again:

```
npm test
```

And you will see:

```
Randomized with seed 29903
Started
F

Failures:
1) the landing page displays the page title
  Message:
    AssertionError [ERR_ASSERTION]: 'Express' deepEqual 'The Friendly Greeting Generator'
  Stack:
```

Oh no! Not again! Go back and check... yup, you definitely changed the name of the app. What could be wrong?

You need to restart your server in your other shell. Exit with _Ctrl-C_ and restart with `npm start`. _(Yes, there is a much better way of doing this)._

Having restarted your application, execute the tests again with `npm test`. You will see this:

```
Randomized with seed 46658
Started
.


1 spec, 0 failures
Finished in 0.071 seconds
Randomized with seed 46658 (jasmine --random=true --seed=46658
```

Awesome. Your first test passes. Recall the stated purpose of this app:

> I want to be able to load my app in a browser, enter my name in an input field, press _Submit_, and receive a friendly greeting in return.

Using this _user story_ as a guide, you can proceed writing your tests. So far, the first part of the story has been covered (i.e., _I want to be able to load my app in a browser_). Now to test the rest...

```
  // ...
  // Add these below our first test in `indexSpec.js`

  it('renders an input form', () => {
    browser.assert.element('input[type=text]'); 
    browser.assert.element('input[type=submit]'); 
  });

  it('returns a friendly greeting if you enter your name and press Submit', done => {
    browser.fill('name', 'Dan');
    browser.pressButton('Submit', () => {
      browser.assert.text('h3', 'What up, Dan?');
      done();
    });
  });

  it('trims excess whitespace from the name submitted', done => {
    browser.fill('name', '                Dawson               ');
    browser.pressButton('Submit', () => {
      browser.assert.text('h3', 'What up, Dan?');
      done();
    });
  });


  it('gets snarky if you forget to enter your name before pressing Submit', done => {
    browser.fill('name', '');
    browser.pressButton('Submit', () => {
      browser.assert.text('h3', 'Whatevs...');
      done();
    });
  });

  it('gets snarky if you forget to enter a blank name before pressing Submit', done => {
    browser.fill('name', '         ');
    browser.pressButton('Submit', () => {
      browser.assert.text('h3', 'Please don\'t waste my time');
      done();
    });
  });
});
```

You can push this as far as you want. For example, you might want to ensure your audience doesn't enter a number or special characters for a name. The ones above define the minimal test-coverage requirement in this case.

Make sure these new tests fail by executing `npm test`. You won't need to restart the server until you make changes to your app _(yes, you should find a better way to manage this)_.

# Make the tests pass

You should try doing this yourself before you skip ahead. I'll give you a couple of clues and then provide the spoiler. In order to get these tests to pass, you'll need to add a route to `routes/index.js` and you'll need to modify the document structure in `views/index.ejs`.

## Did you try it yourself?

### Here's one possible answer:

Make `routes/index.js` look like this:

```
var express = require('express');
var router = express.Router();

/* GET home page. */
router.get('/', function(req, res, next) {
  res.render('index', { title: 'The Friendly Greeting Generator', message: '' });
});

router.post('/', function(req, res, next) {
    
  let message = 'Whatevs...'
  if (req.body.name.length) {

    let name = req.body.name.trim();

    if (!name.length) {
      message = 'Please don\'t waste my time';
    }
    else {
      message = `What up, ${name}?`;
    }
  }

  res.render('index', { title: 'The Friendly Greeting Generator', message: message });
});


module.exports = router;
```

Make `views/index.js` look like this:

```
<!DOCTYPE html>
<html>
  <head>
    <title><%= title %></title>
    <link rel='stylesheet' href='/stylesheets/style.css' />
  </head>
  <body>
    <h1><%= title %></h1>
    <p>Welcome to <%= title %></p>
    <form action="/" method="post">
      <label for="name">Name:</label>
      <input name="name" type=input" />
      <button type="submit">Submit</button>
    </form>

    <% if (message) { %>
      <h3><%= message %></h3>
    <% } %>
  </body>
</html>
```

Note the _EJS_ _alligator tags_ (`<% %>` and `<%= %>`).

When all expectations are satisfied, you will see something like this:

```
Randomized with seed 17093
Started
.....


5 specs, 0 failures
Finished in 0.106 seconds
Randomized with seed 17093 (jasmine --random=true --seed=17093)
```

# What's next?

Got a test question? What kind of app should I build and test?

If you learned something, support my work through [Wycliffe](https://www.wycliffe.ca/projects/dan-bidulock/) or [Patreon](https://patreon.com/whatdandoes).


