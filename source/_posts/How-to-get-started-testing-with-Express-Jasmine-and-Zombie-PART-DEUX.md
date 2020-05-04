---
title: 'How to get started testing with Express, Jasmine, and Zombie - PART DEUX'
date: 2020-05-04 10:54:39
tags:
  - express
  - jasmine
  - zombie
  - testing
---

This post wraps up some loose ends left over from [Part Un](/2020/04/20/How-to-get-started-testing-with-Express-Jasmine-and-Zombie/). If you haven't already, go back and read that now.

After much admirable effort, my buddy Dawson came back at me with a couple of questions from my last tutorial. Namely,

1. Is there a better way to set up the project so you don't have to restart the server every time you change your code?
2. What's supposed to be committed to `git` so that the project can easily be regenerated?

I didn't anticipate Question 2, so I'll start with that...

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

The answer to the question of _What get's committed to Git?_ is simple in this case... _everything but the `node_modules` directory_.

When your project was first initialized, you ran `npm install` to retrieve all the software dependencies listed in `package.json`. These dependencies were downloaded from the [npm repository](https://www.npmjs.com/) and stored in `node_modules/`. From your project directory, execute `ls -al node_modules` to see all the software required by your app. There's quite a lot, even for something as simple as what's been built so far. You definitely _do not_ want to commit all this software to `git` or GitHub. In all likelihood, all those packages are already stored on GitHub, so there's no need to save them to your project.

If you're astute, you'll have noticed a new file created when you first ran `npm install`: `package-lock.json`. This file contains a list of all the dependencies listed in `package.json`, all the dependencies of those dependencies, and the specific versions required by each. That's why there way more files in `node_modules/` than what's listed in `package.json`. Though it will be regenerated every time you run `npm install`, `package-lock.json` should be committed to `git` as well.

### Initialize Git

All this discussion of what gets committed to `git` is silly if your project isn't currently under `git` version control. This is easy to initialize from your project directory:

```
git init
```

If you execute `ls -al` you'll see a directory that wasn't there before: `.git/`. Don't mess around with this directory. Feel free to look, but don't change anything inside.

Though this isn't meant to be a tutorial on `git`, you'll need to set an `origin` repository if you want to `push` your changes to a computer somewhere else in the world. In this case, I'll use [GitHub](https://github.com/), which is a commercial `git` provider. You'll need to create an empty repository to which to push your project. Once that is complete, you can set the new address provided as `origin` in your local project directory. For me, this would look something like this:

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

### Create `.gitignore`

The `.gitignore` file is a special file used by `git` to determine which parts of your project to include and exclude. Create `.gitignore` like this:

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

The `node_modules` directory is gone. It is being _ignored_ by `git`. As you can see, all of these files are _untracked_. That is, they are not under version control. The fastest way to remedy this situation is like this:

```
git add .
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

This means that all your changes are now under version control, but your project still isn't on GitHub. You'll need to do one last thing:

```
git push --set-upstream origin master
```

If you haven't configured TLS certs, you'll be asked for your GitHub username and password. 


