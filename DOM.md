# Document Object Model
- Tree-like representation of the <b>contents</b> of a web page.
- Tree of <b>nodes</b> with different relationships depending on HTML arrangement.

<div id="container">
  <div class="display"></div>
  <div class="controls"></div>
</div>
<div id="container">
  <div class="display"></div>
  <div class="controls"></div>
</div>
<div id="container">
  <div class="display"></div>
  <div class="controls"></div>
</div>
<div id="container">
  <div class="display"></div>
  <div class="controls"></div>
</div>
```html
<div id="container">
	<div class="display"></div>
	<div class="controls"></div>
</div>
```
- <b>Display</b> div is a child of <b>container</b> div 
- <b>Display</b> div is a sibling to <b>controls</b> div
- Think of it as a family tree <b>container</b> is the parent with its children the next level, each with their own <b>branch</b>.

# Targeting Nodes with Selectors
- When working with the <b>DOM</b> you use <b>selectors</b> to target the <b>nodes</b> you want to work with.
- Can use a combination of CSS-style selectors and relationship properties to target nodes.

## CSS-style selectors
- In above example you could use below selectors to refer to <b>Display</b> div
	- `div.display`
	- `.display`
	- `#container > .display`
	- `div#container > div.display`

## Relational selectors
- Can also use relational selectors, with special properties owned by the nodes.
	- `firstElementChild`
	- `lastElementChild`


```javascript
// selects the #container div (don't worry about the syntax, we'll get there)
const container = document.querySelector("#container");

// selects the first child of #container => .display
console.dir(container.firstElementChild);

// selects the .controls div
const controls = document.querySelector(".controls");

// selects the prior sibling => .display
console.dir(controls.previousElementSibling);
```

You identify a certain node based on its relationships to the nodes around it.

# DOM methods
- When HTML is parsed by a browser, it is converted to DOM.
- Primary differences being nodes are JavaScript objects with many properties and methods attached to them.
- Properties and methods are the primary tools we are going to use to manipulate our web page with JavaScript.

### Query selectors
- `element.querySelector(selector)` - returns a reference to the first match of selector.
- `element.querySelectorAll(selectors)` - returns a "NodeList" containing references to all of the matches of the selectors.

querySelectorAll does not return an array. It looks like an array, and it somewhat acts like an array, but it's really a "NodeList". The distinction being several array methods are missing from NodeLists. One solution, if problems arise, is to convert the NodeList into an array with `Array.from()` or the spread operator.

### Element creation
- `document.createElement(tagName, [options])` - creates a new element of tag type `tagName`. `[options]` in this case means you can add some optional parameters to the function. don't worry about these at this point.

```javascript
const div = document.createElement("div");
```

This function does NOT put your new element into the DOM - it creates it in memory. This is so that you can manipulate the element (by adding styles, classes, ids, text, etc.) before placing it on the page. You can place the element into the DOM with one of the following methods.

#### Append elements
- `parentNode.appendCHild(childNode)` - appends `childNode` as the last child of `parentNode`.
- `parentsNode.insertBefore(newNode, referenceNode)` - inserts `newNode` into `parentNode` before `refereceNode`
#### Remove elements
- `parentNode.removeChild(child)` - removes `child` from `parentNode` on the DOM and returns a reference to child.
#### Altering elements 
When you have a reference to an element, you can use it to alter the element's own properties. This allows you to make useful alterations, like adding, removing or altering attributes, changing classes, adding inline style information and more.
```javascript
// creates a new div referenced in the variable 'div'
const div = document.createElement("div");
```

#### Adding inline style
```javascript
// adds the indicated style rule to the element in the div variable
div.style.color = "blue";

// adds several style rules
div.style.cssText = "color: blue; background: white;";

// adds several style rules
div.setAttribute("style", "color: blue; background: white;");
```

When accessing a kebab-cased property like `background-color` with JS, you need to either use camelCase with dot notation or bracket notation. When using bracket notation, you can use either camelCase of kebab-case, but the property name must be a string.

```javascript
// dot notation with kebab case: doesn't work as it attempts to subtract color from div.style.background
// equivalent to: div.style.background - color
div.style.background-color;

// dot notation with camelCase: works, accesses the div's background-color style
div.style.backgroundColor;

// bracket notation with kebab-case: also works
div.style["background-color"];

// bracket notation with camelCase: also works
div.style["backgroundColor"];
```

#### Editing attributes
```javascript
// if id exists, update it to 'theDiv', else create an id with value "theDiv"
div.setAttribute("id", "theDiv");

// returns value of specified attribute, in this case "theDiv"
div.getAttribute("id");

// removes specified attribute
div.removeAttribute("id");
```


#### Working with classes
```javascript
// adds class "new" to your new div
div.classList.add("new");

// removes "new" class from div
div.classList.remove("new");

// if div doesn't have class "active" then add it, or if it does, then remove it
div.classList.toggle("active");
```

It is often standard (and cleaner) to toggle a CSS style rather than adding and removing inline CSS.

#### Adding text content
```javascript
// creates a text node containing "Hello World!" and inserts it in div
div.textContent = "Hello World!";
```

#### Adding HTML content
```javascript
// renders the HTML inside div
div.innerHTML = "<span>Hello World!</span>";
```

Note: Using `textContent` is preferred over `innerHTML` for adding text, as `innerHTML` should be used sparingly to avoid potential security risks.

Your JavaScript, for the most part, is run whenever the JS file is run or when the script tag is encountered in the HTML. If you are including your JavaScript at the top of your file, many of these DOM manipulation methods will not work because the JS code is being run _before_ the nodes are created in the DOM. The simplest way to fix this is to include your JavaScript at the bottom of your HTML file so that it gets run after the DOM nodes are parsed and created.

Alternatively, you can link the JavaScript file in the `<head>` of your HTML document. Use the `<script>` tag with the `src` attribute containing the path to the JS file, and include the `defer` keyword to load the file _after_ the HTML is parsed, as such:

```html
<head>
  <script src="js-file.js" defer></script>
</head>
```


# Events
- Events are how you manipulate the DOM dynamically or on demand.
- Events are things that happen on a webpage like mouse-clicks or key presses

Three primary ways to do this:
- You can specify function attributes directly on your HTML elements.
- You can set properties in the form of `on<eventType>`, such as `onclick` or `onmousedown`, on the DOM nodes in your JavaScript.
- You can attach event listeners to the DOM nodes in your JavaScript.

Event listeners are the preferred method.