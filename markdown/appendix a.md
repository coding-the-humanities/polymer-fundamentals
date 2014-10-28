Appendix A: Setting up your environment
=======================================

After following this appendix, you should be prepared to follow along with the rest of the book - minimally prepared, admittedly, but that's part of Polymer's charm: you can be designing things that work with very little preamble. (By comparison.) Feel free to install more than specified here, but install any less and things won't work.

You can read through the whole thing for an understanding of why you're doing what you're doing (complete with asides for additional but non-essential information), or you could [skip to the nutshell](#tldr).

## Step 0: Git and the command line

You'll interact with almost everything that happens behind the scenes of your projects from the *command line*, *console* or *terminal* - the thing you probably recognize from a thousand and one Hollywood misrepresentations as being the exclusive purview of elite hackers. It looks something like this:

`C:/>_`

Or this:

`you@your-computer ~/$`

Whatever system you have, you should have some form of command line, and you'll want to have it easily accessible, so create a shortcut (or whatever works for you).

> <font size="2">As with any cultural myth, there's a grain of truth underneath the idea that the command line is forbidden magic and people who use it are wizards - it lets you give commands directly to your system, without silly things like a desktop or graphical interface intervening, and you theoretically have a lot more options available to you - so it's both useful for people who know what they're doing and potentially dangerous for people who don't. (Relatively dangerous, that is - you might delete your entire operating system, but you won't accidentally start World War III. Suffice it to say that you're given just enough rope to shoot yourself in the foot.)</font>

**Git** is a *version control* system - it manages your files by keeping track of what's new, what's changed and what's been removed, allowing you to make snapshots of moments in your project's development and then go back to them later (if, say, a feature you added turns out to break everything horribly and you changed too many files for the "undo" button to be a reliable fix). It also supports collaboration and backups by allowing you to upload your code to an online location that mirrors your project exactly (including all the aforementioned snapshots) and lets you choose who else can contribute. It's a ridiculously useful tool, and it's required to install some things you'll need later, so go ahead and grab it from [here](http://git-scm.com/ "Git").

> <font size="2">You may also want to create a <b><a href="https://github.com/">GitHub</a></b> account for yourself - GitHub is the place where collaboration and backups happen, and with which Git interfaces. You don't need to do this to follow along with the rest of the book, and getting your local Git installation to recognize your GitHub account can be a pain, but you're almost guaranteed to need it at some point in the future, so if you plan to do more serious coding (which we're writing this book in the hope you do!) and time is not currently one of your problems, we suggest getting it out of the way now.</font>

If you're given the option to install **Git Bash** or something similar, we suggest doing so. This will give you a command line that mimics the one you'd find on a Linux system, which can simplify your life a little, because
1. a lot of guides to things you might need in future are written in the assumption you're running Linux
2. it gives you that little bit more control over your file system - you can create a blank file with any or no extension with just a few keystrokes, for instance.

However, you don't need Git Bash to follow along with the rest of this book - the commands to the things you're about to install should work from your native command line, whatever that is.

## Step 1: Install Node.js

**Node.js** is the platform on which everything else runs, and it includes a utility for acquiring the other parts - **npm**, which doesn't stand for **node package manager** but might as well do. This means that, comparatively, it's the hardest part to install (even though it's not very hard), because there's no one there to pull it up by its bootstraps.

> <font size="2">Technically, you could download (or *clone*) a Git repository containing all the code you'd need to build your own Node.js installation from scratch, but we figure that if you were comfortable enough with that sort of thing to try it, you wouldn't be reading this book.</font>

Fortunately, [Node's website](http://nodejs.org/ "Nodejs.org") is fairly intelligent about detecting what operating system you have and serving you the correct file when you click the big green "Install" button. There's a [Downloads page](http://nodejs.org/download/ "Downloads") for if you know the particulars of your system better than you trust the site to figure out, but it shouldn't be necessary.

> <font size="2">During the install process, you may be given the option of additionally installing a Node command prompt or similar - it starts up an interactive session into which you can type JavaScript code and have it evaluated on the fly. Feel free, but you won't need it for the purposes of this book.</font>

If, after installing a new package, you try to use it and your command line yells at you, try closing it and re-opening the terminal. What's probably going on is that the installation added new information to your system, telling it how to locate and use whatever you just installed, but your command line was run in a system that didn't yet have that information. Re-starting the terminal will make sure it's up to date.

## Step 2: Install Bower and node-static

**Bower** is a package manager like npm, but for specific projects or applications - in addition to doing more-or-less what npm does, it looks for a special file you create that lists all the packages your project requires and installs what you don't already have. These are referred to as *dependencies*: to say Polymer *depends on* `platform.js` is to say that

- telling a package manager or similar tool to install Polymer means you're also telling it to install `platform.js` for you, because
- Polymer won't work properly (or at all) if `platform.js` isn't installed.

> <font size="2">Technically, npm does this too, but everything we're installing using npm, we want you to have available no matter what project you're working on, whereas you might not need Polymer in every project. Having said that, if you think installing a massive chain of utilities to code something ostensibly simple is a little silly, you're not alone - and getting started with Polymer is a comparatively lightweight operation, go figure. This is how the cookie crumbles, however, and will likely remain so for the foreseeable future.</font>

**node-static** is a pre-fabricated package that sets up a simple web server (with some optional parameters you're free to forget about for the time being) using whatever resources (files) are in the directory from which it's invoked.

> <font size="2">You're going to need this because your web browser is just a *client* - it can read and display the web pages it receives, but it has to receive them from somewhere. Normally, that somewhere is "the Internet", but if you want to see something you just coded you'd have to set up an account with a hosting service and jump through all kinds of hoops. It's much simpler to just point your browser back at your own computer, but the data has to be presented in just the right way: it has to be *served*. That's what node-static does.<br>
For very simple HTML documents, it's possible to just open the file in a browser from your hard drive, but anything much more complex than that will rapidly refuse to work. Hence, a simple server.</font>

Both Bower and node-static are installed using npm. To the command line! Enter this in your terminal and hit Enter:

`npm install -g bower node-static`

> <font size="2">The `-g` flag stands for "global", and will ensure that npm makes both packages available from anywhere. You could also install both packages using separate `npm install -g` commands, but there's no real need to.</font>

## Step 3: Get Polymer

**Finally.** Polymer is installed with Bower, which works similarly to npm. As mentioned earlier, however, instead of making its packages available system-wide, Bower saves information about a project's packages in a file, aptly named `bower.json`.

> <font size="2">`.json` stands for JavaScript Object Notation, and it's worth reading up on, since it's a common way to pass data between web applications and it's based on JavaScript, which we'll be using fairly extensively while coding Polymer. Again, though, it's not strictly critical at this point.</font>

First, create a folder for your project and navigate to it: the command `mkdir folder` will create a folder named `folder` inside the current working directory (which is indicated before every command prompt if you're on Windows, and can be seen with the command `pwd` - "Print working directory" - on Mac or Linux). Then, `cd folder` will navigate to the newly-created folder. (You can, of course, name it something other than "folder".) Now, you could create your `bower.json` here by hand, or you could let Bower do it for you. Just type into your command line:

`bower init`

and you'll get a series of questions, which you can comfortably ignore by pressing Enter to accept all the default options.

Now, Bower knows that you're starting a project, so the next command will have the desired effect:

`bower install --save Polymer/polymer`

This will download the basic Getting-Started-With-Polymer Kit and put it in a folder, inside your project's main folder, called `bower_components`; the `--save` flag will make sure the dependency on whatever you're installing is noted in your `bower.json`.

## Step 4: Serving a basic index page

That basic Getting-Started-With-Polymer Kit includes two things: Polymer itself, and `platform.js`, a library containing *polyfills* - consider these the glue that holds the smooth underbelly of Polymer together with the differently-curved edges of the various browsers which users might ask to display it.

> <font size="2">The general idea is that eventually, every browser will support Polymer (and other Web Component standards) "right out of the box"; until then, however, polyfills will be necessary to provide whatever features Polymer needs but a given browser doesn't yet provide.</font>

We mentioned that your browser is a client which displays pages sent by a server; if you want to see your code in action, you're going to have to create a page for the node-static server to serve, and it's going to have to include both of the components in the Kit.

The first thing to do, then, is to create a file named `index.html`. On Linux, Mac or using Git Bash from Windows, this is as simple as

`touch index.html`

but if you're using a Windows command line you're going to have to create it elsewise (by opening a text editor and saving a new file, for instance).

> <font size="2">If you don't already have a favorite text editor, we recommend <a href "http://www.sublimetext.com/">Sublime Text</a>.</font>

We're assuming you're familiar with the basic structure of a HTML document, so just copy-paste this one:

```html
<!DOCTYPE html>
<html>
    <head>
    </head>
    <body>
    </body>
</html>
```

> <font size="2">If you want quick confirmation later on that your server is working, add something in between the `<body>` tags. ("Hello, world" is both standard and predictable.)</font>

All that's left is to include Polymer and the platform; of the two, the platform has to be included first, since Polymer is `import`ed using functionality that might not be native to the browser displaying it. In fact, it's a good idea to make sure that the following line is always the very first line in your `index.html`'s `<head>` section:

`<script src="./bower_components/platform/platform.js"></script>`

> <font size="2">The `.` before the path to `platform.js` tells the browser that it should start looking for the first location in the path from the same directory as `index.html`. This is the expected, sensible thing, but expected and sensible are not always the same thing as the default behavior.</font>

Right after that line, include this one:

`<link rel="import" href="./bower_components/polymer/polymer.html"/>`

The `import` value for the `rel` attribute won't work if `platform.js` isn't correctly loaded; if your page loads but you don't see your elements, check your `<head>` section, because it's likely to be the culprit. Once you've included these lines, you're ready to start your server and get to coding Polymer. Go back to your command line and enter:

`static`

You should see something like

`serving "." at http://127.0.0.1:8080`

127.0.0.1, as you may be aware, is like your computer's first-person singular pronoun: it's how it refers to itself. 8080 is the specific port on which your server is now listening for connections. You can reach it by pointing your browser at the address your terminal is showing you, or by pointing it at `localhost:8080`, which refers to the same thing.

**Congratulations!** You're ready to move on to the book proper. Just remember to leave the terminal open so it continues running your server, and to refresh the page in your browser any time you make changes to your files.

## tl;dr: Start coding Polymer in five minutes <a name="tldr"></a>

* Get Git from http://git-scm.com/
* Get node.js from http://nodejs.org/
* Open command prompt/terminal
    * `npm install -g bower node-static`
    * `cd your/project/directory`
    * `bower init`
    * `bower install --save Polymer/polymer`
* Create a new file `index.html`
* Open `index.html` in your favorite text editor and add the following lines:
    * `<!DOCTYPE html>`
    * `<html>`
    * `<head>`
    * `<script src="./bower_components/platform/platform.js"></script>`
    * `<link rel="import" href="./bower_components/polymer/polymer.html"/>`
    * `</head>`
    * `<body></body>`
    * `</html>`
    * Save the file
* Back to the command line:
    * `static`
* Your server is running (point your browser at `localhost:8080` by default) and your index page is ready for you to start coding your first Polymer element.


