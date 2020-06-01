---
title: Simple Photo Blogging for Partnership Development
tags:
- tutorial
- express
- testing
- jasmine
- zombie
---

God has blessed me. I'm always sure to ask Him to guide my hand and direct me in how to use the time I've been given. Toward that end I asked God, _What should I do today?_ He told me to start building a simple photo-blogging application.

I've actually been thinking about a photo-blog app for a long time. There are a lot of motivations behind it (even apart from God's will, I'm afraid), but having been released to the opportunity, I plan to make the most of it.

There's a place along the road in Cochrane, Alberta. My family pass it on our way to church. A sign warning _Danger, Open Excavation_ has stood guard over tarps for almost a year. The site is either an archeological dig or a crime scene. Maybe both. It's near the Cochrane Ranche historic site. Every time we pass, I think _Hmmmmm, I'd love to know what's going on over there_.

<!-- more -->

And why can't I? The people involved are probably excited about their work. How cool would it be if everyone on the dig could snap photos and even take notes so that people like me can go to their website to see what's up?

Sharing progress on an archeological dig is just one possible application. Whether I'm writing software for SIL, building a rocket stove, or replacing a tap on my rainbarrels, I've long felt the need for a simple app that gets pictures from my workspace onto a website. Not only would it provide a way to keep track of what I'm doing for my own benefit, I may also choose to share what I learned with others. There are many potential applications...

- A gardening log for planting, watering, and growth
- Documenting an approach to a problem
- Collecting photos of hazards on a construction site
- Live, on-scene reporting for journalists
- Disassembly of machines or electronics for repair and reassembly
- A private photo-sharing app for families
- Espionage

Apart from obedience to God and a personal inclination toward treachery, the app and the accompanying tutorial series are mostly movitated by my work with SIL International. There, my team and I have been tasked with providing identity management services for our growing portfolio of web-based language translation tools. The photo-blog app under construction will likely be the first to demonstrate the use of this service. The accompanying tutorial documents the test-driven approach I took in developing the application. Follow along to learn and, hopefully, improve upon my instruction. If you do learn something, you may support my work through [Wycliffe](https://www.wycliffe.ca/projects/dan-bidulock/) or [Patreon](https://patreon.com/whatdandoes).


## The Basic User Story

No, not like [Trainspotting](https://en.wikipedia.org/wiki/Trainspotting_&lpar;film&rpar;)... I'm actually pretty new to _User Stories_. They basically work like this:

> As a <role\>  
> I want to <goal\>  
> So that I can <reason_benefit\>  
> And can see that <additional_behaviours\>  

So, in the most general terms, this is what I want out of my _photo-blog_:

> As a **guy with no time for blogging**  
> I want to **post photos with notes to my personal website**  
> So that I can **show off all the neat things I do**  
> And can see that **others are super jealous**

As development and the tutorials progress, the user stories will become more specific. They will, in fact, reveal the tests required.

## How to use this Tutorial

If you're brand new to programming, this tutorial is not for you. I am assuming a basic understanding of software and development practices. I'll do my best to stick to core principles and technologies for those who may not be new to programming, but are new to web development.

All that follows was carried out on an Ubuntu 18.04 system with all necessary tools and system dependencies already installed (e.g., `git`, `docker`, `node`, et al).

### Get Set Up

If you have a GitHub account, I recommend forking the tutorial repository and cloning from there. If you prefer to get a quick start, clone it straight from the source:

```
git clone https://github.com/WhatDanDoes/photo-blog.git
```

However you obtain the software, navigate to the project directory and install:

```
cd photo-blog
npm install
```

At this point, the application is ready for testing and development in the local environment. If you follow the tutorial, you need to start from the beginning...

Execute the following:

```
git branch
```

You will see that you are on the `master` branch. In this case, you are currently looking at the project in its _finished_ state. Each installment of the tutorial will have its own series of _branches_. The names of these branches describe their purpose. For example, to view the empty tests for the first tutorial, execute the following:

```
git checkout tutorial/1-landing-page
```

You could start workng directly on this branch, but it's better to start a branch of your own. Most teams will have a branch called `develop`, which is where work in progress is refined until it's ready for `master`. You should currently be on the `tutorial/1-landing-page` branch (run `git branch` if you're not sure). Create your own `develop` branch like this:

```
git checkout -b develop
```

The `-b` creates a new branch if the one named doesn't exist. `develop` is now an exact copy of `tutorial/1-landing-page`. Any changes you now make won't be reflected on the original branch. The tutorial work is cumulative. The intention is that when you complete a step, you'll _merge_ the next `tutorial` branch into `develop` and continue work from there. 
You're almost ready to get started on the first tutorial. Run the tests like this:

```
npm test
```

This executes all the tests in the project's `spec/` directory. They'll all fail, of course, because they're just sketched in. It's your job to fill in the blanks and make them pass.

If you get stuck or aren't sure how to approach a test, commit your work and sneak a peak at the solution branch:

```
git checkout solution/1-landing-page
```

If you run `npm test` in a `solution` branch, all the tests should execute and pass. If you discover a better approach than what you find there, be sure to submit a pull request so others can learn by your example.

Remember how I said this tutorial series isn't for beginners? It's up to you to manage your `git` branches. Make sure you're comfortable with rudimentary `git` before getting started. If you know what you're doing, it is quite alright to take an approach different than the one prescribed.


Tutorial #1, coming soon...



