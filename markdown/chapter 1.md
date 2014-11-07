Chapter 1: Your first element
=============================

In this chapter, you'll be getting your first taste of declarative programming - that thing where you *declare* what you want to happen... *and it happens*.

It's not quite that simple, but almost.

We'll assume that you have your environment all set up - that you have a static web server running with Node.js, serving a simple index page that includes Polymer and `platform.js`. If any of that sounds like alien gibberish, never fear - either head over to [Appendix A](/content/book/c-f-k/polymer-book/appendix_a_setting_up_your_environment/README.html) and follow the instructions there, or (if you're more experienced or feeling particularly confident) hunt up instructions on setting up each component. They're out there, and they go into more detail than you'll need to learn Polymer, which at this point may be too much of a good thing. It's up to you.

Ready? Read on.

## Step 0: What are we building?

We outlined the project in the Introduction, but Rome wasn't built in a day, and neither is a web application. For our first element, we'll build something that we can expand on later.

> <font size="2">This is perfectly in keeping with a declarative style of programming, as well as something called test-driven development (TDD). It works like this: you tell your platform or your program to do something you know it can't do, and make sure it fails, as a sort of preliminary reality check. Then, you tell the machine how to do it and check again. Then, just rinse and repeat, the tests gradually increasing in complexity until your project does everything you want it to.</font>

The first thing we want to do is use Polymer's *templating* and *data-binding* functionality, which respond dynamically to user input without us having to do any mucking around with JavaScript. So, we'll create a simple input text box that writes its contents to the page as output.

## Step 1: Declare your element

Add the following to the `<body>` section of your `index.html`:

`<universal-tag-machine></universal-tag-machine>`

It's that simple.

Of course, it doesn't *do* anything yet (but we expected that). Next, let's fix that problem.

## Step 2: Make it work

Polymer adds only a little bit of functionality on top of the HTML you're already familiar with. The first thing you'll want to do is make your browser understand what you mean by `universal-tag-machine`, by passing that as the value of the `name` attribute in a `polymer-element` element.

> <font size="2">The `import`ed `polymer.html` ensures the browser knows what to do with an element like that, and `platform.js` ensures the import itself is successful (among other things).</font>

Add the following to the `<body>` section. The name, of course, can be whatever you need it to be, but for Polymer to recognize it as a custom element, the name has to include at least one hyphen.

> <font size="2">Of course, the name you choose should reflect what your element actually does, so that if someone else has to read and understand (or, perish the thought, *fix*) your code, they won't waste valuable time figuring out which part needs their attention. Code that exhibits this property is commonly called *maintainable* code, and you should strive for it. In fact, our choice of `universal-tag-machine` isn't the greatest in this regard - we'd advise you to stick with it to minimize confusion (and because we like it), but also think about what might make a more maintainable name.</font>

The `noscript` attribute should be included, too - Polymer won't work correctly if you omit a `<script>` tag but don't explicitly *declare* that you won't be using any JavaScript.

```html
<polymer-element name="universal-tag-machine" noscript>
</polymer-element>
```

If you save your page and reload it in the browser, and then use the developer console to examine the page source, you should see the `<universal-tag-machine>` element more or less where you expect it; it just won't have any content yet. So, add the following inside the `<polymer-element>`:

```html
<template>
    <input value="Resistance is futile"></input>
    <h1>Resistance is futile</h1>
</template>
```

This is the basic format of all Polymer elements: the HTML you know and love, inside a pair of `<template>` tags so Polymer knows what to insert into the element when it's declared.

> <font size="2">When we move on to using CSS and JavaScript, remember that the `<style>` tags go *inside* the `<template>`, while the `<script>` tags go *outside* it.</font>

Our problem now is that the page always displays "Resistance is futile", no matter what you enter into the text box. We need a *variable* value for both the `value` attribute of the `<input>` element and the text content between the `<h1>` tags, and we need to make the one reflect the other dynamically.

With Polymer, this couldn't be simpler:

```html
<template>
    <input value="{{ message }}"></input>
    <h1>{{ message }}</h1>
</template>
```

That's it! Variables declared this way are considered *bound* by Polymer, which will do the hard work of ensuring that any double pair of curly braces (or handlebars, or mustaches, whichever name you prefer) which contain the same variable name also all contain exactly the same data. (Within the same element, of course - you could declare a second `<universal-tag-machine>` element and the content of its `<h1>` tags would only reflect the content of its own `<input>` element.)

> <font size="2">There's a good reason for this, and it has some further implications, which we'll get into in later chapters.<br><br>Also, a brief note on formatting: notice how the first `{{ message }}` is inside quotation marks? This is because the only thing Polymer really does here is to play mad libs with your data - it doesn't affect how the browser understands HTML. The attributes of an HTML element need to be inside quotation marks, so if your data is contained in an attribute somewhere (like `value`) then that entire binding should be inside quotation marks too.</font>



## Recap

What just happened? You:
- **declared your element** - told the browser what you wanted and where you wanted it, then
- **defined your element** - told the browser how to give you what you wanted. In the process, you used
    - **templating** - you defined a basic, reusable structure that your user can fill in later, and
    - **data binding** - you made sure that when the user changed one value, all its associated values changed along with it - even if they were contained inside elements the user can't usually modify.

This is what your `index.html` file should look like, start to finish:

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
        <input value="{{ message }}"></input>
        <h1>{{ message }}</h1>
      </template>
    </polymer-element>

  </body>
</html>
```

**Congratulations!** You just wrote your first custom HTML element. And the best part is - you didn't have to write a single line of JavaScript in order to make it respond dynamically in a useful way to user input. We'll get our hands dirty with scripting in just a moment, but first - you might be wondering whether you have to define every custom element in the same document within which it's declared. After all, you `import`ed `polymer.html` itself, didn't you? If you asked yourself this question, give yourself a cookie, because you're absolutely right. A very important skill to learn (and learn early) is **organizing your code**. But to get the most out of that lesson, it's useful to have a little more code to organize - especially code of different kinds. For that reason, the next thing we'll do is to **combine multiple elements**.
