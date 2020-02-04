---
title: Lab 1
subtitle: ''
date: 2020-02-04T18:34:52.620Z
thumb_img_path: /images/960x0.jpg
content_img_path: /images/960x0.jpg
template: post
---
## Assignment 1

> Find the third `<p>` and give that a red font colour.

First off we have to select the third `<p>`. If we look inside the HTML file we will find this:

```
<div id="assignment1">
  <p>First</p>
  <p>Second</p>
  <p>Third</p>
  <p>Forth</p>
</div>
```

All the tags are inside of a `div` with the id of `assignment1`. In order to get the one we need first have to select all of them.

```
let paragraphs = document.querySelectorAll('#assignment1 p')
```

We now have a `NodeList`(a collection of elements from the DOM, **not an array**, but we can access the elements inside it or iterate through them just like we would with an array) that holds all the paragraph tags and we can select the third one just like we would select the third element in an array.

```
thirdParagraph = paragraphs[2]
```

Now that we have our paragraph we only need to change the colour of the text inside of it.

```
thirdParagraph.style.color = 'red'
```

After this line is executed our HTML will look like this:

```
<div id="assignment1">
  <p>First</p>
  <p>Second</p>
  <p style="color: red">Third</p>
  <p>Forth</p>
</div>
```

We can of course combine all of the above into a single line.

```
document.querySelectorAll('#assignment1 p')[2].style.color = '#f00'
```





