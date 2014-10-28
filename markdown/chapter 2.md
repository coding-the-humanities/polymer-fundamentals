Chapter 2: Combining multiple elements
======================================

In the last chapter, you familiarized yourself with a sizeable chunk of Polymer's core functionality. Rather than rush off immediately into the wild unknown again, in this chapter you'll see a few ways in which what you already know can be (usefully and interestingly) applied to itself.

## Step 0: What are we doing?

Going back to our idea for a Universal Tag Machine, it should be clear without too much soul-searching that the smallest meaningful units of content the user is presented with are *images*. Of course, it's a fair bit more complex under the hood, but the simplest thing a user will eventually do with our component is:
 - input a URL
 - have it displayed as the image it refers to.

So, the first thing we should do is make sure it can do that. In the process we'll be taking advantage of Polymer's predilection for *encapsulation* and the concurrent need for effective *message passing*. In addition, because the Tag Machine eventually has to display a lot of content in a repetitive way, we'll take in a little bit of *iteration*.

> <font size="2">You'll notice that these chapters have been starting off with a Step 0 describing what the chapter's goals are. For one thing, this is supposed to be a tutorial, and it's a tried-and-true teaching method to give your learner an idea of what they're aiming for so they'll know intuitively what it looks like when they reach it. <br><br>Also, though, if you do any designing in the future, you'll find this is an invaluable strategy: do the lion's share of your structural thinking up-front, so that when you run into problems, you can be confident that your approach might be flawed but your concept isn't. Especially in programming languages which don't encourage this behavior - <i>we're looking at you, Perl</i> - there's often nothing more frustrating than realizing too late you're trying to do something that was a bad idea in the first place. That's always going to remain a possibility, of course, but the more time you spend getting the idea right before you do any coding, the smaller it will be.</font>

## Step 1: From `message` to `url`

It might seem like a mostly superficial, semantic change, but remember we're programming declaratively - we *declare* what we want first and make the machine understand it later. (Technically speaking, you could leave the data-bound variable the way it is, of course, but then your code wouldn't be maximally reflective of what it actually does. That's bad for anyone who has to look at your code after you're done writing it - including you, if you come back to it later.) So, change the value inside the curly braces to reflect the kind of data we expect it to refer to:

```html
<input value="{{url}}"></input>`
<p>{{url}}</p>
```

Of course, this doesn't actually make the element behave any differently - so let's get rid of the `<p>` tags and put something else in their place.

What, though? The answer is staring you in the face: if you have a variable that refers to an image URL, you need an element that knows what to do with that kind of data.

```html
<img src="{{url}}"></img>
<img src="{{url}}"></img>
<img src="{{url}}"></img>
```

Go ahead and add a couple copies of the same tag - it's implicit in using multiple elements that we'll eventually have to style them relative to each other, so we might as well get that under our belts early too.

> <font size="2">Deciding on an `<img>` tag might seem like an incredibly trivial step to explain in such detail, but you should get in the habit of thinking of your data-bound variables as blocks that fit only into holes of the correct shape. This idea is important for reasons which we'll get into later in this chapter.</font>

## Step 2: Adding style

As you might have expected, adding CSS to a Polymer element is as simple as adding `<style>` tags to your `<template>`. Make your images thumbnail-sized and tell them to display vertically:

```html
<polymer-element name="universal-tag-machine" noscript>
  <template>
    <style>
      img {
        display: block;
        width: 100px;
      }
    </style>
    <input value="{{url}}"></input>
    <img src="{{url}}"></img>
...
```

> <font size="2">Why does the `<style>` tag go inside the template while the `<script>` tag (to which we are, by degrees, coming) goes outside it? This behavior is hard-coded, deeply enough that it doesn't matter whether or not you think it's sensible because it's just how things are, but there <i>is</i> a reason for it, and it's worth thinking about.</font>

## Step 3: Simplify, simplify, simplify

There's already kind of a lot going on in our element. You probably have no problem keeping it all straight in your head right now, but imagine you're working on something more complex - projects can quickly grow beyond the ability of one person to keep track of.

The solution is something called *encapsulation* - the actual, structural separation of parts of your project that do functionally different things. There are probably as many ways to implement this as there are programming languages (or more); Polymer's is to make sure that different elements are tightly sealed off from one another.

Well, perhaps not *totally* sealed. Elements should have ways to "talk" to each other, or it'll be difficult to do much with user input. Think of it as similar to the protein-synthesizing enzymes in your cells: they have "active sites" that only fit certain molecules, on which they only perform certain functions, and they completely ignore all the other stuff floating around the cell. Polymer elements have active sites of their own: any time you need to pass data in or out of an element, you can give it custom *attributes* similar to the ones baked into the HTML you know.

> <font size="2">There's a school of thought which holds that all programs should be written this way - with their component parts as black boxes which only communicate with other parts in tightly controlled, predictable ways. It's called <i>functional programming</i>, and we'll be getting into it later on for a variety of reasons, one of which being that it makes code easier to reason about and test.</font>

The next thing we'll do, then, is take advantage of this *message passing* by separating some functionality into a new element - specifically, the styling - and having the two talk to each other.

> <font size="2">Why separate out the styling particularly, and not, say, the input box, which is arguably as different from the displayed content as the styling is from the HTML? The answer is because appearance (CSS), structure (HTML) and behavior (JavaScript) are separate concerns, and parts of your project which handle them should be kept as separate as possible also. This <i>separation of concerns</i> is the flip side of encapsulation; the idea in both cases is to keep a clear idea of who's responsible for what.</font>

The process for this new element is exactly the same as for the old one. First of all, declare it - you're going to make an element which handles the styling of the images, so remove the `<style>` and `<img>` tags and replace the latter with something else. (We're calling it `<utm-thumbnail>` - remember, the name has to contain at least one hyphen.)

```html
<polymer-element name="universal-tag-machine" noscript>
  <template>
    <input value="{{url}}"></input>
    <utm-thumbnail></utm-thumbnail>
    <utm-thumbnail></utm-thumbnail>
    <utm-thumbnail></utm-thumbnail>
  </template>
</polymer-element>
```

Then, define it, somewhere inside the `<body>`, and add what you just took out of your first element:

```html
<polymer-element name="utm-thumbnail" noscript>
  <template>
    <style>
      img {
        display: block;
        width: 100px;
      }
    </style>
    <img src="{{url}}"></img>
  </template>
</polymer-element>
```

You might have noticed a fairly serious problem - ``{{url}}`` refers to the `value` of the `<input>` element in `<universal-tag-machine>`, and `{{url}}` also refers to the `src` of the `<img>` element in `<utm-thumbnail>`, but as we explained earlier, data-bound variables are strictly contained within their elements, so those two instances of `{{url}}` now definitely don't refer to the same data.

What's the solution? Message passing, of course. Custom attributes of Polymer elements are declared in the same line of code in which you declare the element itself:

```html
<polymer-element name="utm-thumbnail" attributes="url" noscript>
```

> <font size="2">There's actually another way to declare them, but it involves (and is most useful for) scripting, so we'll run into it later.</font>

You just gave your `<utm-thumbnail>` element an active site for talking to its environment - in this case, the `<universal-tag-machine>` element. In terms of function, you just told `<utm-thumbnail>` that it should fill its instance of `{{url}}` with whatever is passed into the `url` attribute of the element by its environment.

The next thing, then, should be to actually give your declared elements that attribute. (Active sites don't do anything if molecules don't drift into contact!)

```html
<polymer-element name="universal-tag-machine" noscript>
  <template>
    <input value="{{url}}"></input>
    <utm-thumbnail url="{{url}}"></utm-thumbnail>
    <utm-thumbnail url="{{url}}"></utm-thumbnail>
    <utm-thumbnail url="{{url}}"></utm-thumbnail>
  </template>
</polymer-element>
```

There's potential here for some confusion to arise, since we're using `url` to refer to several different but related things. It's currently
- the value of the `<img>` element's `src` attribute
- a custom attribute which is part of the definition of `<utm-thumbnail>`
- the same custom attribute when it's actually declared
- the value of the same custom attribute on each of the three instances of `<utm-thumbnail>`
- the value of the `value` attribute on the `<input>` element.

Change the custom attribute to `source` for the sake of experiment.

```html
<polymer-element name="utm-thumbnail" attributes="source" noscript>
```

Which other values should change to make sure everything still works? This one:

```html
<utm-thumbnail source="{{url}}"></utm-thumbnail>
```
And this one:
```html
<img src="{{source}}"></img>
```

See the relationship? Attributes declared in a given element's definition become *both* data-bound variables *within* that element *as well as* non-negotiable access points from *outside* it. (If this ties your brain in knots, feel free to keep the name `source` so you remember what refers to what.)

> <font size="2">We introduced substantial complexity in this step, and yet the name "Simplify, simplify, simplify" wasn't intended ironically. Things like this require non-trivial effort to get your head around if you're encountering them for the first time, but once you've done that work, they'll drastically simplify the process of coding as well as the process of understanding code you didn't (recently) write. And, of course, you'll notice that both of your elements are now much simpler in design than the old element which tried to fulfill both tasks!</font>

## Step 4: Don't repeat yourself

Something that might still be bugging you at this point is the fact that you have three separate instances of `<utm-thumbnail>` inside `<universal-tag-machine>`. You might think that repetition is something best handled by machines, and you might suspect that there's a way to get Polymer to do this particular bit of repetition for you, and you'd be absolutely correct in thinking both of those things.

"Don't Repeat Yourself" is a fairly fundamental principle of programming - it's the same reason why Bill Gates is on record as saying he'd rather hire a lazy coder than a smart one, because the lazy one will figure out how to get the job done with the least possible amount of effort. Similarly, not having to repeat yourself endlessly is sort of the whole point of Polymer's templating functionality. So, it shouldn't come as much of a surprise that that's exactly what we're going to use to solve this problem.

See, if a `<template>` can tell Polymer what to repeat each time the element is declared, there's no reason it can't be used to repeat something more often than once per declaration; the only additional information it needs is exactly how often to repeat. Polymer's `repeat` syntax, passed as an attribute of a template, provides for exactly that. Modify your `<universal-tag-machine>` element until it looks like this:

```html
<polymer-element name="universal-tag-machine" noscript>
  <template>
    <input value="{{url}}"></input>
      <template repeat="{{number in [1,2,3]}}">
         <utm-thumbnail url="{{url}}"></utm-thumbnail>
      </template>
  </template>
</polymer-element>
```

The biggest change, of course, is the `{{number in [1,2,3]}}`, and there's a lot of information in those few characters. The first thing you should know is that, although it looks like just a slightly more complex data-bound variable, it isn't quite. There are actually two variables there; what the curly braces in `repeat="{{x in y}}"` are saying can be translated as something like "for every item in `y` (which I expect to be an ordered list), I'll do everything inside this template once, and each time I'll refer to the list item I'm using as `x`".

> <font size="2">There's a much shorter and easier way to say "a program going through a list and doing something repetitive for every piece of data it finds". The process is called <i>iterating</i>, such that you can speak of "iterating over" a list; each separate run through the list is called an <i>iteration</i>. This is usually more informally referred to as "looping over" or "looping through", but in conversational English that doesn't carry the additional connotation of doing something repetitive each time, which is pretty important in this context.</font>

Furthermore, in this particular case, it doesn't actually matter what we're using on either side of the `in`. First off, `[1,2,3]` is what's referred to as a *primitive* - one of the basic data types a programming language recognizes.

> <font size="2">Specifically, the square brackets indicate it's an <i>array</i> - JavaScript's way of saying "a simple list in which the order of items matters". We'll be getting to know them much better in later chapters.</font>

The more common use of `repeat` is to pass in a list that actually has something to do with the content of the template - which you'd do by using the list's data-bound name in the place we're currently using `[1,2,3]` (minus the curly braces, of course - they're already there). For our purposes, though, it doesn't matter right now what the items are, just that there are three of them. It could be `[3,2,1]`, or `["stars","cat","space"]`, et cetera. Similarly, because we don't actually use the numbers, `number` could be anything else and the template would still work as intended. Usually, though, we'd be telling the template to do something with each list item being iterated over, and it would treat the placeholder name as yet another data-bound variable, like this:

```html
<template repeat="{{number in [1,2,3]}}">
  <p>{{number}}</p>
</template>
```

Phew. That's a lot of information packed into a small space. But now we have one element that displays whatever image is referred to by the URL it's fed, and another element that takes a URL and uses the previous element to display three images. Except... it'd make sense to collect these images into a container of their own, since the Tag Machine is going to do this with a lot of different images eventually, and make it display them horizontally, since horizontal screen real estate is more abundant than vertical. And really, iteration is functionally separate from input and output...

## Step 5: You gotta keep 'em separated

It should be starting to feel familiar by this point: we need a new element, and it should contain all the functionality we're about to factor out of the element that currently contains it. First, declare it, and make sure it has an attribute for you to pass in the input URL:

```html
<polymer-element name="universal-tag-machine" noscript>
  <template>
    <input value="{{url}}"></input>
    <utm-image-row url="{{url}}"></utm-image-row>
  </template>
</polymer-element>
```

Next, define it (we suggest doing so in between the two you already have) and add the code you're separating:

```html
<polymer-element name="utm-image-row" attributes="url" noscript>
  <template>
    <template repeat="{{number in [1,2,3]}}">
      <utm-thumbnail url="{{url}}"></utm-thumbnail>
    </template>
  </template>
</polymer-element>
```

> <font size="2">We've gone back to using `url` as our go-to name for all the different variables and attributes here - we trust that you understand what refers to what and where the data's going, but if you don't yet, feel free to rename things until you have a clear picture of what's going on. (You'll almost certainly break it in the process, and we encourage you to - you'll probably learn more intuitively from fixing it than we could ever explain in words.)</font>

What about making the `<utm-image-row>` into an actual row? There are lots of ways to do that, of course, including more `<style>` tags, but orientation and layout are such common issues that we might as well show you Polymer's `layout` attributes (which at the time of writing are a recent addition!). They're attributes that can be added to any element inside a Polymer element and they'll generally do what they sound like they'll do (the more programming languages you become familiar with, the more you'll realize this is both rare and pretty hard to accomplish). For now, all we need is the following:

```html
<template>
  <section horizontal layout>
    <template repeat="{{number in [1,2,3]}}">
      <utm-thumbnail url="{{url}}"></utm-thumbnail>
    </template>
  </section>
</template>
```

`layout` clues Polymer in that there's work to be done, and `horizontal` makes every element inside its parent orient itself horizontally with respect to other such elements. (We've put them in a `<section>` rather than further modify our `<template>`s to keep the code clean and separate our concerns: templates are for structure, and layout is part of appearance.)

> <font size="2">This functionality isn't actually part of Polymer, strictly speaking - it's an implementation of `flexbox`, a CSS standard which tries harder than the old box model to accommodate changing display sizes. The limited Polymer documentation says that Polymer "exposes" the flexbox syntax; this means that it's baked into Polymer by default and you can access it without too much trouble.<br><br>This concept of <i>exposure</i> is related to encapsulation and message passing - it's true in the same sense that your `<utm-thumbnail>` element <i>exposes</i> the `src` attribute of the `<img>` element it contains. It can be helpful when fashioning elements to work together to think about what each of them needs to expose (and we'll be dealing more directly with this concept when we get to scripting).</font>

## Recap

In this chapter, you:
- **Made your element display an image for a URL**,
- **styled that image**,
- learned about **separation of concerns** and why it's a good idea to have that styling done by a separate element,
- used Polymer's `<template repeat>` syntax to create a bunch of images with fairly little work, learning about **iteration** in the process,
- created not one but two separate elements in the process, learning about **encapsulation and message passing**.

Here's what your `index.html` file should look like:

```html
<!DOCTYPE html>
<html>
  <head>
    <script src="./bower_components/platform/platform.js"></script>
    <link rel="import" href="./bower_components/polymer/polymer.html"/>
  </head>
  <body>

    <universal-tag-machine></universal-tag-machine>

    <polymer-element name="universal-tag-machine" noscript>
      <template>
        <input value="{{url}}"></input>
        <utm-image-row url="{{url}}"></utm-image-row>
      </template>
    </polymer-element>

    <polymer-element name="utm-image-row" attributes="url" noscript>
      <template>
        <section horizontal layout>
          <template repeat="{{number in [1,2,3]}}">
            <utm-thumbnail url="{{url}}"></utm-thumbnail>
          </template>
        </section>
      </template>
    </polymer-element>

    <polymer-element name="utm-thumbnail" attributes="url" noscript>
      <template>
        <style>
          img {
            display: block;
            width: 100px;
          }
        </style>
        <img src="{{url}}"></img>
      </template>
    </polymer-element>

  </body>
</html>
```

This is more or less the extent of what you can do with Polymer without using `<script>` tags - and you'd be surprised how many projects you can get done without writing a single line of JavaScript, so you shouldn't read that as implying a Polymer element is incomplete without some scripting. But Polymer itself has a lot more to offer; we'll just need to go a little deeper down the rabbit hole to get to it. For that reason, the next chapter will be concerned with writing **simple scripts**.
