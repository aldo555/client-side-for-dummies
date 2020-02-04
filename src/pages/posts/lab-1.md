---
title: Lab 1
subtitle: ''
date: 2020-02-04T18:34:52.620Z
thumb_img_path: /images/960x0.jpg
content_img_path: /images/960x0.jpg
template: post
---
# Assignment 1

> Find the third `<p>` and give that a red font colour.

First off we have to select the third `<p>`. If we look inside the HTML file we will find this:

```html
<div id="assignment1">
  <p>First</p>
  <p>Second</p>
  <p>Third</p>
  <p>Forth</p>
</div>
```

All the tags are inside of a `div` with the id of `assignment1`. In order to get the one we need first have to select all of them.

```javascript
let paragraphs = document.querySelectorAll('#assignment1 p')
```

We now have a `NodeList`(a collection of elements from the DOM, **not an array**, but we can access the elements inside it or iterate through them just like we would with an array) that holds all the paragraph tags and we can select the third one just like we would select the third element in an array.

```javascript
thirdParagraph = paragraphs[2]
```

Now that we have our paragraph we only need to change the colour of the text inside of it.

```javascript
thirdParagraph.style.color = 'red'
```

After this line is executed our HTML will look like this:

```javascript
<div id="assignment1">
  <p>First</p>
  <p>Second</p>
  <p style="color: red">Third</p>
  <p>Forth</p>
</div>
```

We can of course combine all of the above into a single line.

```javascript
document.querySelectorAll('#assignment1 p')[2].style.color = '#f00'
```

# Assignment 2

> Append a child element for each `<div>` with a class of child.

The HTML looks like this: 

```html
<div><p>I'm a div</p></div>
<div class="child"><p>I'm a div with a class of child</p></div>
<div class="child"><p>I'm a div with a class of child</p></div>
<div><p>I'm a div</p></div>
```

To start we have to select all the elements that have the class of child.

```javascript
let futureParents = document.querySelectorAll('.child')
```

Since there is more than one element selected, we need to iterate through them all and for that we will use a `for` loop.

```javascript
for (let i = 0; i < futureParents; i++) {
  // create a new element
  // put some text inside it for good measure
  // append it to the parent
}
```

We will now create a new element and place some text inside of it.

```javascript
let child = document.createElement('p')
child.innerText = 👶
```

A new `<p>` tag has just been born, now we need to append it to our parent which is `futureParents[i]`, the current element in our iteration.

```javascript
futureParents[i].appendChild(child)
```

Let's put everything together.

```javascript
let futureParents = document.querySelectorAll('.child')

for (let i = 0; i < futureParents; i++) {
  let child = document.createElement('p')
  child.innerText = 👶
  futureParents[i].appendChild(child)
}
```

# Assignment 3

> Find the `<ul>` with an id of sub-nav and traverse up the DOM until you find a `<ul>` with a class of primary-nav and give it a font-size of 20px.

First off, this is what our HTML looks like: 

```html
<ul class="primary-nav">
    <li>First</li>
    <li>Second</li>
    <li>Third
        <ul id="sub-nav">
            <li>First sub</li>
            <li>Second sub</li>
            <li>Third sub</li>
        </ul>
    </li>
    <li>Forth</li>
    <li>Fifth</li>
</ul>
```

Let's start by selecting the element with the id of sub-nav.

```javascript
let subNav = document.querySelector('#sub-nav)
```

Now we can traverse up the DOM by using the `closest` function. What this does is it looks for the closest element that meets a certain criteria(has a specific tag, class, id) by going up in the DOM tree(from parent to parent).

```javascript
let primaryNav = subNav.closest('.primary-nav')
```

And for the font-size:

```javascript
primaryNav.style = 'font-size: 20px'
```

Let's put all this together in one line.

```javascript
document.querySelector('#sub-nav').closest('.primary-nav').style = 'font-size: 20px'
```

# Assignment 4

> Remove all *childs* of any `<div>` with a class of remove.
