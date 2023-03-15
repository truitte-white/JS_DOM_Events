### JSDR 213

# DOM EVENTS

![](https://res.cloudinary.com/practicaldev/image/fetch/s--J43C-IwA--/c_imagga_scale,f_auto,fl_progressive,h_900,q_auto,w_1600/https://dev-to-uploads.s3.amazonaws.com/i/1zm80qaekzgu8p54w98g.jpg)

## Lesson Overview

So far, we've seen the HTML skeleton of our websites. We've had a basic introduction to CSS, and learned about two incredible frameworks to use with them Flexbox and Grid. We've taken a dive into Javascript working with our different data types, conditional logic, loops, and functions, and discussed this thing called the DOM that allows us to override our CSS and HTML with JS. 

But user interactivity doesn't work well if we're just doing Console Logs and inspecting things with the Dev Tools. 

In this lesson, we'll learn all about DOM events and how we can use them. We'll also learn about callback functions and the power we can wield by using them.

By the end of this lesson, you'll be able to add real functionality to our HTML pages.

## Getting Started

- Fork this lesson
- Open this folder in VsCode
- Create an HTML, CSS, and JS file

## Lesson objectives

_After this lesson, students will be able to:_

1. Describe what a browser event is
1. Create a click event
1. Use a named, referenced function as the click handler for the listener

## Describe what a browser event is

Every kind of interactivity in the browser is an event: clicks, mouseovers, key presses, scrolling, resizing, loading the page, and more.

When you interact with the browser it checks to see if there is a _listener_ for that interaction.

If there is a _listener_ present, the browser will try to run any _handlers_ for those interactions.

A _handler_ is just a function that runs a desired procedure.

## Create a click event

How can we set up a _click_ event?

We need:

1. An element to set it on
2. A _listener_ that listens for the event: on _which element_ should the event take place
3. A _handler_ that runs the procedure we want to have happen when the event is triggered

Make a button in the html:

```html
<button id="btn">Click me</button>
```

Grab the button in the JS (DOM element):

```javascript
const btn = document.getElementById('btn')
console.log(btn)
```

### Event listener

Set an event listener:

Use `addEventListener()` [.addEventListener() documentation](https://developer.mozilla.org/en-US/docs/web/api/eventlistener)

```javascript
btn.addEventListener('click')
```

The event listener takes a string as an argument. There are just a few strings that it will recognize as valid events, and 'click' is one of them.

[List of events](https://developer.mozilla.org/en-US/docs/Web/Events)

### Event handler

Add a _function_ that runs what we want to have happen. This function is what _handles_ the event and is called an _event handler_:

```javascript
btn.addEventListener('click', () => {
  console.log('Element clicked through function!')
})
```

Notice that we have supplied a function as an argument. The jargon for using a function as an argument to another function is `callback`.

Pseudocode for an event listener

```javascript
elem.on(STRING, CALLBACK)
```

### Add Text to the Page on Click

```javascript
btn.addEventListener('click', () => {
  document.body.append('I have been clicked!')
})
```

## Use a named, referenced function as the click handler for the listener

The _handler_ that we used for our click was _anonymous_. It was a function that had no name. We just told the listener to run an _anonymous_ function. We can give our function a name and thereby reuse that function with other event listeners.

### Named Function

We can abstract the anonymous function out and give it a name:

Separate function, not inside the listener:

```javascript
const addText = () => {
  document.body.append('I have been clicked!')
}
```

We can then reference it in the event Listener:

```javascript
btn.addEventListener('click', addText)
```

With a named function, we can use the same handler for more than one DOM element.


But there are some serious limits to this 'document.body.append' method, as we can only target one thing at a time. Lets take it to the next step. Create a text element in your HTML, give it an ID, and leave it blank

```html
<h2 id="title> </h2>
```    


Now lets update our function to specifically add text to this element by working with the InnerText method of DOM Manipulation!


```javascript
const addText = () => {
  let title = document.querySelector("#title")
  title.innerText = 'I have been clicked!'
}
```
Now we can style and work with these elements as we'd like. We can even make these dynamic with our other JS methods


```javascript

let name = prompt('what is your name?')

const addText = () => {
  let title = document.querySelector("#title")
  title.innerText = `hello ${name}!`
}
```

### Referenced Function

Note that we do not invoke the function with parentheses. We do not want to invoke the function right away, we merely want to _reference_ it to be invoked when the listener runs it.

- The function should be defined before it is used in the event listener
- When the function is invoked inside the event listener **leave out the parentheses**. We do not want to invoke the function right away! We merely want to reference that function in the listener.

Here the function is invoked and will run immediately:

```javascript
btn.addEventListener('click', addText())
```

We don't want this! We only want the function to run when the user has clicked on the button.

Complete code:

```javascript
const btn = document.getElementById('btn')

const addText = () => {
  document.body.append('It seems as if it has been clicked!')
}

btn.addEventListener('click', addText)
```

Let's do something fancier, and toggle the background-color of the page using `.toggleClass()`

```javascript
const changeClass = () => {
  document.body.classList.toggle('red')
}

btn.addEventListener('click', changeClass)
```

CSS:

```css
.red {
  background-color: red;
}
```


Lets take this a few steps further working with some other CSS properties, including Display, Font Color, and others!



### Add Additional HTML

Let's add the following HTML to between the `<body>` tags in `index.html`:

```html
<h3>Comments</h3>
<ul>
  <li>SEI Rocks!</li>
</ul>
<input>
<button>Add Comment</button>
```

### Listen to the `click` Event on the `<button>`

We can add a `click` event listener to pretty much any element - not just buttons.  However, buttons are pre-styled to look and act clickable 😃

We're going to use an anonymous callback function in this first example:

```js
// Select the button
const btn = document.querySelector('button');
btn.addEventListener('click', function(evt) {
  // testing!
  console.log(evt);  
});
``` 

If all goes well, clicking the button should log out the **event object**.

Congrats, attaching your first event listener!

<details>
<summary>
❓ What's the name of the method used to attach event listeners to elements?
</summary>
<hr>

**`addEventListener()`**

<hr>
</details>

<details>
<summary>
❓ Name three events that might be triggered in the browser.
</summary>
<hr>

**`click`, `submit`, `change`, `mousemove`, etc.**

<hr>
</details>

## 5. The Event Object

Examining the **event object** that was provided as an argument to our event listener reveals lots of useful information about the event!

Of special interest are:

- Several `...X` and `...Y` properties that provide where the click occurred.

- The `target` property, which holds a reference to the DOM element that triggered (dispatched) the event.

The `target` property will certainly be the one to remember as it will be used very often moving forward.

## 6. Adding a New Comment

### Application Logic

When we click the `[Add Comment]` button, we'll need our event listener function to do the following:

1. Create a new `<li>` element
2. Set the `<li>` element's `innerText` to that of the text entered in the `<input>`
3. Append the `<li>` to its parent, the `<ul>`
4. Finally, clear the text from the `<input>`

### Creating a New `<li>` Element

If we want to add a new comment to the DOM, we're going to need to create a new `<li>` element.

Here's how we can do it using the `document.createElement` method:

```js
btn.addEventListener('click', function(evt) {
  // Create a new <li> element
  const newCommentEl = document.createElement('li');
  console.log(newCommentEl)
});
```
> Note: At this point, the element is "in memory" only and is not part of the DOM (yet).

Okay, we have a new `<li>` element created and assigned to a variable named `newCommentEl`, but it has no content...

### Getting/Setting the Text of an `<input>`

We'll need to access the text the user has typed into the `<input>` element in order to set the `innerText` of `newCommentEl`.

Since `<input>` elements are **empty elements**, i.e., elements with no content, `innerText` and `innerHTML` properties do not exist.

Instead, we get and set the text within an `<input>` using the `value` property.

> NOTE: The `value` property maps to the `value` attribute in HTML.

Because this app needs to access the `<input>` every time the `<button>` is clicked, it would be more efficient to cache (a developer term for 'remember' or 'store') the `<input>` outside of the event handler function so that it isn't selected over and over:

```js
// Cached elements
const inputEl = document.querySelector('input');

btn.addEventListener('click', function(evt) {
  const newCommentEl = document.createElement('li');
  // Access the input's text
  const commentText = inputEl.value;
  console.log(commentText)
});
```

🤔 For future reference, remembering that we use the `value` property to get/set the text of an `<input>` element is worthwhile.

### Setting the Content of the `<li>`

So, now we can set the `innerText` of the new `<li>`:

```js
// Cached elements
const inputEl = document.querySelector('input');

btn.addEventListener('click', function(evt) {
  const newCommentEl = document.createElement('li');
  const commentText = inputEl.value;
  // Set newComment's text
  newCommentEl.innerText = commentText;
});
```

Now the new `<li>` is ready to be added to the DOM...

### Adding the `<li>` to the DOM

<details>
<summary>
❓ Which element do we we want to add the <code>&lt;li&gt;</code> to?
</summary>
<hr>

**The `<ul>`**

<hr>
</details>

There are a few ways to add DOM elements to the document using JavaScript.

One common way to add new elements to another element is by using the [append()](https://developer.mozilla.org/en-US/docs/Web/API/Element/append) method like this:

```js
// Cached elements
const inputEl = document.querySelector('input');
// Remember the <ul> too!
const ulEl = document.querySelector('ul');

btn.addEventListener('click', function(evt) {
  const newCommentEl = document.createElement('li');
  const commentText = inputEl.value;
  newCommentEl.innerText = commentText;
  // Add the new li as the ul's last child
  ulEl.append(newCommentEl);
});
```

Test it out - nice!

### Clearing the Text in the `<input>`

The new comment has been added, but if we want to improve the UX, we have one more task - clear out the `<input>`.

### 👉 YOU DO: Improve the UX (1 min)


## Lesson Recap

In this lesson, we learned all about event listeners and handlers and how we can leverage them. We also learned about the all-important callback function and hwo to use it.

## Resources

- [Build a Drum Kit in Vanilla JS](https://www.youtube.com/watch?v=VuN8qwZoego)
- [Event reference](https://developer.mozilla.org/en-US/docs/Web/Events)
- [Introduction to events](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Building_blocks/Events)
- [Eloquent JavaScript: Handling Events](http://eloquentjavascript.net/14_event.html)
- [Philip Roberts: What the heck is the event loop anyway?](https://www.youtube.com/watch?v=8aGhZQkoFbQ)
