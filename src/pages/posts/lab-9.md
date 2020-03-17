---
title: Lab 9
date: 2020-03-16T23:35:55.336Z
thumb_img_path: /images/guyfieri-babyyoda-header.jpg
content_img_path: /images/guyfieri-babyyoda-header.jpg
template: post
---
## The video

Because of its complexity and the amount of stuff I have to explain I have decided to do a video guide for this lab. You can still find all the code you need in this post but for in depth explanations you have to [follow this link](https://google.com).

## The HTML

At the time of solving this lab, the example was very buggy and I had to work around it. The HTML file was probably changed since then to get rid of the bugs. Here is the file I used: 

```html
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Lab 9 - AJAX</title>
    <link rel="stylesheet" href="./lab9_username.css">
</head>
<body>
    <div class="content--container">
        <h1>Grade 3</h1>
        <div class="users--container">
            <h2>Users</h2>
            <div id="users_response"></div>
            <button id="get_users">Get Users</button>
        </div>
        
        <div class="posts--container">
            <h2>Posts</h2>
            <div id="posts_response"></div>
            <button id="get_posts">Get Posts</button>
        </div>

        <div class="selected_user_post--container">
            <h2>Get post by selected user</h2>
            <select name="users" class="users_dropdown"></select>
            <button id="get_user_post">Get user post</button>
        </div>

        <h1>GRADE 4</h1>
        <div class="create_post--container">
            <h2>Create new post</h2>
            <form action="POST" id="create_post_form">
                <label for="">Select user</label>
                <select name="users" class="users_dropdown"></select>
                <label for="">Title</label>
                <input type="text" name="post_title" id="post_title">
                <label for="">Body text</label>
                <textarea name="body" id="post_body"></textarea>
                <button id="create_post">Create new post</button>
            </form>

            <div class="created_post--container">
                
            </div>
        </div>

        <h1>GRADE 5</h1>
        <div class="update_post--container">
            <h2>Update Post</h2>
            <form action="POST" id="update_post_form">
                <input type="hidden" id="update_id" value="">
                <input type="hidden" id="update_user_id" value="">
                <label for="">Title</label>
                <input type="text" id="update_post_title">
                <label for="">Body</label>
                <textarea name="update_body" id="update_body"></textarea>
                <button id="update_post">Update post</button>
            </form>

            <div id="updated_post--container"></div>

        </div>

    </div>
    <script src="./lab9_username.js"></script>
</body>
</html>
```

## The Javascript

```javascript
let posts = []

async function getUsers () {
  const users = await fetchUsers()
  renderUsers(users)
  populateUsersDropdowns(users)
}

async function getPosts () {
  posts = await fetchPosts()
  renderPosts()
}

async function getUserPost () {
  const userId = document.querySelectorAll('.users_dropdown')[0].value

  const post = await fetchUserPost(userId)
  posts.push(post)
  renderPosts()
}

async function createNewPost (e) {
  e.preventDefault()

  const userId = document.querySelectorAll('.users_dropdown')[1].value
  const title = document.querySelector('#post_title').value
  const body = document.querySelector('#post_body').value

  const newPost = await fetchNewPost(userId, title, body)
  posts.push(newPost)
  renderPosts()

  document.querySelector('#post_title').value = ''
  document.querySelector('#post_body').value = ''

  const createdPostContainer = document.querySelector('.created_post--container')
  createdPostContainer.innerHTML = ''

  const notification = document.createElement('h2')
  notification.textContent = `Your post "${newPost.title}" has been created`
  createdPostContainer.appendChild(notification)
}

async function updatePost (e) {
  e.preventDefault()

  const postId = document.querySelector('#update_id').value
  const userId = document.querySelector('#update_user_id').value
  const title = document.querySelector('#update_post_title').value
  const body = document.querySelector('#update_body').value

  const updatedPost = await fetchUpdatedPost(postId, userId, title, body)
  
  const postIndex = posts.findIndex(post => {
    return post.id === updatedPost.id
  })

  posts[postIndex] = updatedPost

  renderPosts()

  document.querySelector('#update_id').value = ''
  document.querySelector('#update_user_id').value = ''
  document.querySelector('#update_post_title').value = ''
  document.querySelector('#update_body').value = ''

  const updatedPostContainer = document.querySelector('#updated_post--container')
  updatedPostContainer.innerHTML = ''

  const notification = document.createElement('h2')
  notification.textContent = `Your post "${updatedPost.title}" has been updated`
  updatedPostContainer.appendChild(notification)
}

function selectPostToUpdate () {
  const post = posts.find(post => {
    return post.id === Number(this.dataset.id)
  })

  document.querySelector('#update_id').value = post.id
  document.querySelector('#update_user_id').value = post.userId
  document.querySelector('#update_post_title').value = post.title
  document.querySelector('#update_body').value = post.body
}

async function fetchUsers () {
  const response = await fetch('https://jsonplaceholder.typicode.com/users')
  const data = await response.json()
  return data
}

async function fetchUserPost (userId) {
  const response = await fetch(`https://jsonplaceholder.typicode.com/posts?userId=${userId}`)
  const data = await response.json()
  return data[0]
}

async function fetchPosts () {
  const response = await fetch('https://jsonplaceholder.typicode.com/posts')
  const data = await response.json()
  return [...data.slice(0, 10)];
}

function renderUsers (users) {
  const userContainer = document.querySelector('#users_response')

  users.forEach(user => {
    const name = document.createElement('h3')
    name.textContent = user.name
    userContainer.appendChild(name)

    const email = document.createElement('p')
    email.textContent = user.email
    userContainer.appendChild(email)
  })
}

function populateUsersDropdowns (users) {
  const dropdowns = document.querySelectorAll('.users_dropdown')

  dropdowns.forEach (dropdown => {
    users.forEach(user => {
      const option = document.createElement('option')
      option.textContent = user.name
      option.value = user.id
  
      dropdown.insertAdjacentElement('afterbegin', option)
    })
  })
}

function renderPosts () {
  const postContainer = document.querySelector('#posts_response')
  postContainer.innerHTML = ''

  posts.forEach(post => {
    const title = document.createElement('h3')
    title.textContent = post.title
    postContainer.appendChild(title)

    const body = document.createElement('p')
    body.textContent = post.body
    postContainer.appendChild(body)

    const editButton = document.createElement('button')
    editButton.textContent = 'Edit'
    editButton.dataset.id = post.id
    editButton.dataset['user_id'] = post.userId
    editButton.addEventListener('click', selectPostToUpdate)
    postContainer.appendChild(editButton)
  })
}

async function fetchNewPost (userId, title, body) {
  const response = await fetch('https://jsonplaceholder.typicode.com/posts', {
    method: 'POST',
    body: JSON.stringify({
      userId: userId,
      title: title,
      body: body,
    }),
    headers: {
      "Content-type": "application/json; charset=UTF-8"
    }
  })
  const data = await response.json()
  return data
}

async function fetchUpdatedPost (postId, userId, title, body) {
  const response = await fetch(`https://jsonplaceholder.typicode.com/posts/${postId}`, {
    method: 'PUT',
    body: JSON.stringify({
      id: postId,
      title: title,
      body: body,
      userId: userId
    }),
    headers: {
      "Content-type": "application/json; charset=UTF-8"
    }
  })
  const data = await response.json()
  return data
}


document.querySelector('#get_users').addEventListener('click', getUsers)
document.querySelector('#get_posts').addEventListener('click', getPosts)
document.querySelector('#create_post_form').addEventListener('submit', createNewPost)
document.querySelector('#update_post_form').addEventListener('submit', updatePost)
document.querySelector('#get_user_post').addEventListener('click', getUserPost)
```



