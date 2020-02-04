---
title: Lab 6
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
for (let i = 0; i < futureParents.length; i++) {
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

for (let i = 0; i < futureParents.length; i++) {
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

The HTML:

```html
<div>
    <p>I'm a div</p>
</div>
<div class="remove">
    <p>I'm a div with a class of remove</p>
</div>
<div class="remove">
    <p>I'm a div with a class of remove</p>
</div>
<div>
    <p>I'm a div</p>
    <div class="remove">
        <p>I'm a div with a class of remove</p>
    </div>
</div>
```

You can notice in the code above that all the divs with the class of remove have a single child that is a paragraph so we can go ahead and select them.

```javascript
let toBeKilled = document.querySelectorAll('.remove p')
```

To remove them we have to iterate over all of the elements that are on death row and use the function `remove`.

```javascript
for (i = 0; i < toBeKilled.length; i++) {
  toBeKilled[i].remove()
}
```

# Assignment 5

> Find the `<div>` with the class hidden and remove that class to find out what's inside it.

HTML looks like this:

```html
<div class="item_not_hidden hidden">
    <img src="img/hidden.jpg" alt="">
</div>
```

Of course we start by selecting the element with the class hidden.

```javascript
let hiddenElement = document.querySelector('.hidden')
```

Now that we have it we can call upon the `classList` property which helps us deal with adding, removing or toggling classes.

```javascript
hiddenElement.classList.remove('hidden')
```

Everything on one line:

```javascript
document.querySelector('.hidden').classList.remove('hidden')
```

# Assignment 6 - Grade 4

> Create a filtering menu. The active filters should have a bolder font when clicked. The all filter shall only be bald when no other filters are selected and can't be bold when any other filters are selected.

Our HTML: 

```html
<div>
    <span id="all" class="filter-item active">All</span>
    <span class="filter-item">Jackets</span>
    <span class="filter-item">Trousers</span>
    <span class="filter-item">Hats</span>
    <span class="filter-item">Shoes</span>
</div>
```

This one is a little bit more complicated so let's begin by figuring out the logic of it all. There are 2 possibilities here, you can click on All and in this case every other filter goes back to normal and All becomes bold or you can click on any of the 4 filters in which case the one you clicked on becomes bold and the All filter goes back to normal.

You might ask yourself by now how do you get the element that is clicked. This is where `this` come into play. In the case of an event such as a click, `this` refers to the element that received the event, meaning the one that is clicked.

One more thing, the boldness is added by the class active which pre-existent.

Now that we have an understanding of the problem let's begin solving it. Since we have two possibilities we have to use a conditional statement, an 'if'.

```javascript
if (this.id === 'all') {
  // case when user has clicked the All button
}
else {
  // case when user has clicked any other button
}
```

In the first scenario when the user has clicked the All button, we need to make all other buttons go back to normal and the All button bold. We need to start off by selecting all the buttons excluding the All button of course.

```javascript
let buttons = document.querySelectorAll('.filter-item:not(#all)')
```

As you can tell we have used a bit of a strange syntax in order to exclude the All button but there is no need to remember it, just be ready to google it.

To remove the boldness from all these buttons we need to remove the active class from all of them and we will do that by using a `for` loop and the `classList` property which we used in the previous assignment.

```javascript
for (let i = 0; i < buttons.length; i++) {
  buttons[i].classList.remove('active')
}
```

And now to add the active class to the All button:

```javascript
document.querySelector('#all').classList.add('active')
```

We are all done with the first case! Now in case the user has clicked on a button other then All we have to remove the active class from the All button and toggle the active class on the element that was just clicked.

```javascript
document.querySelector('#all').classList.remove('active')
this.classList.toggle('active')
```

If we put everything together it should look like this:

```javascript
if (this.id === 'all') {
  let buttons = document.querySelectorAll('.filter-item:not(#all)')
  for (let i = 0; i < buttons.length; i++) {
    buttons[i].classList.remove('active')
  }
  document.querySelector('#all').classList.add('active')
}
else {
  document.querySelector('#all').classList.remove('active')
  this.classList.toggle('active')
}
```

# Assignment 7 - Grade 5

>Make all the cats or dogs movable from one place to another.

HTML, of course: 

```html
<div>
  <div class="first">
    <h3>First shelter</h3>
    <p class="cat">😸</p>
    <p class="cat">😸</p>
    <p class="dog">🐶</p>
  </div>
  <div class="second">
    <h3>Second shelter</h3>
  </div>
  <div class="third">
    <h3>Third shelter</h3>
  </div>

  <button id="first_to_second_all" class="btn">Move all from first to second</button>
  <button id="first_to_third_all" class="btn">Move all from first to third</button>
  <button id="second_to_first_all" class="btn">Move all second to first</button>
  <button id="only_cats_first_to_second" class="btn">Move only cats from first to second</button>
  <button id="only_dogs_second_to_third" class="btn">Move only dogs from second to third</button>
</div>
```

You should be happy to know this is a lot easier than assignment 6. I will only be covering one of the functions(move all from first shelter to the second shelter) since after that you can just copy and paste with very little changes.

I'd like to start by adding the event listener for the click event.

```javascript
document.querySelector('#first_to_second_all').addEventListener('click', firstToSecondAll)
```

Now that the button is all hooked up we need to define our function.

```javascript
function firstToSecondAll() {
  // animal movement
}
```

We should select the animals from the first shelter and then also select the second shelter so we can later append them to it.

```javascript
let firstShelterAnimals = document.querySelectorAll('p')
let secondShelter = document.querySelector('.second')
```

In order to append the animals to the second shelter we need to iterate through them and append them one by one using a `for` loop.

```javascript
for (let i = 0; i < firstShelterAnimals.length; i++) {
  secondShelter.appendChild(firstShelterAnimals[i])
}
```

Our function now looks like this: 

```javascript
function firstToSecondAll() {
  let firstShelterAnimals = document.querySelectorAll('p')
  let secondShelter = document.querySelector('.second')

  for (let i = 0; i < firstShelterAnimals.length; i++) {
    secondShelter.appendChild(firstShelterAnimals[i])
  }
}
```

I hope you got the idea by now since the other functions will only require minimal changes.
