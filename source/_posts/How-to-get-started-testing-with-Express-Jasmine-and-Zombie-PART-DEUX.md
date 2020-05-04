---
title: 'How to get started testing with Express, Jasmine, and Zombie - PART DEUX'
date: 2020-05-04 10:54:39
tags:
  - express
  - jasmine
  - zombie
  - testing
---

This post addresses a question arising from [Part Un](/2020/04/20/How-to-get-started-testing-with-Express-Jasmine-and-Zombie/). If you haven't already, go back and read that now.

After much admirable effort, my buddy Dawson came back at me with a question from my last tutorial. Namely,

> What's supposed to be committed to `git` so that the project can easily be regenerated?

<!-- more -->

That is the focus here...

## What gets committed to Git?

If you've worked through [the first part of this tutorial](/2020/04/20/How-to-get-started-testing-with-Express-Jasmine-and-Zombie/), you'll end up with a directory structure that looks like this:

```
▾ bin/
    www*
▸ node_modules/
▾ public/
  ▸ images/
  ▸ javascripts/
  ▸ stylesheets/
▾ routes/
    index.js
    users.js
▾ spec/
  ▸ support/
    indexSpec.js
▾ views/
    error.ejs
    index.ejs
  app.js
  package-lock.json
  package.json
```

The answer to the question of _What get's committed to Git?_ is simple in this case...  everything _except_ the `node_modules` directory.

When your project was first initialized, you ran `npm install` to retrieve all the software dependencies listed in `package.json`. These dependencies were downloaded from the [NPM repository](https://www.npmjs.com/) and stored in `node_modules/`. From your project directory, execute

```
ls -al node_modules
```

to see all the software required by your app. There's quite a lot, even for something as simple as what's been built so far. You definitely _do not_ want to commit all this software to `git` or GitHub. In all likelihood, those packages are already stored on GitHub, so there's no need to save them to your project.

If you're astute, you'll have noticed a new file created when you first ran `npm install`: that is, `package-lock.json`. This file contains

1. a list of all the dependencies listed in package.json
2. all the dependencies of those dependencies, and
3. the specific versions required by each.

That's why there are many more directories in `node_modules/` than dependencies listed in `package.json`. Though it will be regenerated every time you run `npm install`, `package-lock.json` should be committed to `git` as well.

### Initialize Git

All this discussion of what gets committed to `git` is pointless if your project isn't currently under `git` version control. `git` is easy to initialize from your project directory:

```
git init
```

If you execute `ls -al` you'll see a directory that wasn't there before: `.git/`. Don't mess around with this directory. Feel free to look, but don't change anything inside.

Though this isn't meant to be a tutorial on `git`, you'll need to set an `origin` repository if you want to `push` your changes to a computer somewhere else in the world. For this I'll use [GitHub](https://github.com/), which is a commercial `git` provider. You'll need to create an empty repository to which to push your project on GitHub. Once that is complete, you can set the new repository address provided as `origin` in your local project directory. For me, this would look something like this:

```
git remote add origin https://github.com/WycliffeDan/myapp.git
```

If you do this correctly, you won't see any feedback from the command line. Execute `git remote -v`, to verify the change you just made:

```
origin	https://github.com/WycliffeDan/myapp.git (fetch)
origin	https://github.com/WycliffeDan/myapp.git (push)
```

Execute `git status` and you'll see this:

```
On branch master

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)

	app.js
	bin/
	node_modules/
	package-lock.json
	package.json
	public/
	routes/
	spec/
	views/

nothing added to commit but untracked files present (use "git add" to track)
```

These are the files and directories you want to bring under `git` version control... well, all those _except_ `node_modules/`, remember.

### Create .gitignore

The `.gitignore` is used by `git` to determine which parts of your project to exclude from version control. Create `.gitignore` like this:

```
touch .gitignore
```

Inside that file, list every file and directory you want _ignored_ on a line of its own. For me, a minimal `.gitignore` file would look like this:

```
node_modules/

# I use `vim`, which creates a bunch of backup files. The following entry makes sure all that clutter doesn't go to GitHub
*.sw*
```

_Note:_ I don't, but a lot of people use MacOS. Excluding this file is common: `.DS_Store`. Add it to the end of the list, if you like.

### Commit and push

Execute `git status` again and you'll see something different:

```
On branch master

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)

	.gitignore
	app.js
	bin/
	package-lock.json
	package.json
	public/
	routes/
	spec/
	views/

nothing added to commit but untracked files present (use "git add" to track)
```

The `node_modules` directory is not listed. It's still stored on the file system, but it is being _ignored_ by `git`. As you can see, all of these files are _untracked_. That is, they are not under version control. The fastest way to remedy the situation is like this:

```
git add . # <-- don't miss the period!
```

Now when you run `git status`, you'll see:

```
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)

	new file:   .gitignore
	new file:   app.js
	new file:   bin/www
	new file:   package-lock.json
	new file:   package.json
	new file:   public/stylesheets/style.css
	new file:   routes/index.js
	new file:   routes/users.js
	new file:   spec/indexSpec.js
	new file:   spec/support/jasmine.json
	new file:   views/error.ejs
	new file:   views/index.ejs
```

Your project is just about ready to be pushed to GitHub. All the files are _staged for commit_. `git` is waiting for you to finalize the commit with a helpful note to yourself:

```
git commit -am "Hello, myapp"
```

I always say _Hello_ when I'm adding a new file to my repository. It's a bit of tradition/silly habit. You can start your own tradition.

Execute `git status` again:

```
On branch master
nothing to commit, working tree clean
```

This means that all your changes have been committed and are now under version control, but your project still isn't safely stored on GitHub. You need to do one last thing:

```
git push --set-upstream origin master
```

If you haven't configured TLS certs, you'll be asked for your GitHub username and password. If successful, you'll see a message confirming your project was succesfully _pushed_.

## Documentation

Whoa! Almost forgot. My buddy Dawson wants to be able to easily _regenerate_ his project from his GitHub repository. Do not neglect documenting how to set up and use your app. I cannot tell you how many times even rudimentary instructions have saved my sanity.

It is typical to document setup and deployment steps in a `README.md` file:

```
touch README.md
```

A minimal, barebones `README.md` would look like this:

````
myapp
=====

An introductory test-driven development tutorial project

# Installation

Clone and execute:

```
cd myapp
npm install
```

# Testing

```
npm test
```

# Development Server

```
npm start
```

````


Though it seems easy to remember, do not neglect this step. Keep your `README.md` file up-to-date as you progress in building your app. You'll thank yourself later.

### Commit and push your changes

Execute `git status`:

```
On branch master
Untracked files:
  (use "git add <file>..." to include in what will be committed)

	README.md

nothing added to commit but untracked files present (use "git add" to track)
```

`README.md` is a brand new file, so it's not under version control. Add the file to `git`:

```
git add README.md
```

Check `git status`:

```
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

	new file:   README.md

```

Finalize the commit:

```
git commit -am "Hello, README.md"
```

You can check `git status` again, but I'm just going to push the changes:

```
git push
```

Notice that this time you don't need to execute the more verbose `git push --set-upstream origin master` as you did on the first push thanks to the `--set-upstream` option, which lets `git` know you want to push to GitHub by default.


If you learned something, support my work through [Wycliffe](https://www.wycliffe.ca/projects/dan-bidulock/) or [Patreon](https://patreon.com/whatdandoes).
