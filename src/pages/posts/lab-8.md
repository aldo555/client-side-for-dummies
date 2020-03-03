---
title: Lab 8
subtitle: The bane of my existence.
date: 2020-02-18T20:53:21.963Z
thumb_img_path: /images/madboi.jpg
content_img_path: /images/madboi.jpg
excerpt: The bane of my existence.
template: post
---

#### Check the lab 7 post to see how to get the html and css files.

### Toggling the form visibility

To do this, we have to toggle the class `active` on the form container. Also, don't forget to add the event listener.

```javascript
function toggleFormVisibility() {
    document.querySelector('#form--container').classList.toggle('active')
}

document.querySelector('#form_toggle').addEventListener('click', toggleFormVisibility)
```

### Clear the form inputs

We can do this by setting the value of the inputs to an empty string.

```javascript
function clearFormInputs() {
    document.querySelector('#firstname').value = ''
    document.querySelector('#lastname').value = ''
    document.querySelector('#phonenumber').value = ''
    document.querySelector('#index').value = ''
}
```

### Checking if the inputs are valid

> The form needs to be validated meaning that we do not allow numbers or special characters in neither the first, nor last name and the phone number shall only allow digits 0-9, +, and ().

For more complex validation we have to use **Regular Expressions (RegEx)**. RegEx are used to define search patterns and they might look weird but learning how to use them can be very useful even in areas other than programming. If you don't want to do that you can just Google for them just like I did.

```javascript
function formIsValid() {
    return /^[a-zA-Z]*$/.test(document.querySelector('#firstname').value) && 
        /^[a-zA-Z]*$/.test(document.querySelector('#lastname').value) &&
        /^[+]*[(]{0,1}[0-9]{1,4}[)]{0,1}[-\s\./0-9]*$/.test(document.querySelector('#phonenumber').value)
}
```

Let's go over what is happening in the above code block. 

`/^[a-zA-Z]*$/.test(document.querySelector('#firstname').value)` - here we are checking if the value inside of the first name input matches our RegEx. This expression only allows for the letter a-z and A-Z. If the value inside our input matches this pattern (basically, is valid) then the `test` function will return `true`.

If all of our checks are good and we get back `true` from all of them then our `formIsValid` will also return `true`.

### Getting all the contacts

We are storing all of our contacts inside of the local storage so in order to add, remove or edit a contact we first have to retrieve the list of contacts.

```javascript
function getAllContacts() {
    return JSON.parse(localStorage.getItem('contacts')) || []
}
```

Here I used a little bit of Javascript trickery but in order to understand it let me first show you the equivalent by using an `if` statement.

```javascript
if (JSON.parse(localStorage.getItem('contacts)) {
  return JSON.parse(localStorage.getItem('contacts')
}
else {
  return []
}
```

If we already have the contacts in our local storage we get them, if not we just return an empty array.

`return JSON.parse(localStorage.getItem('contacts')) || []` - This works because of how Javascript evaluates the logical operators (`||`, `&&`). When we use the or logical operator `or` only one of the checks needs to be true for the check to pass. In our case if `JSON.parse(localStorage.getItem('contacts')` has a value then the `or` will be evaluated to true so this will be returned. In the case when the contacts don't exist it will also go on to the second part of the logical operation and `[]` will be returned.

This is called a **short-circuit evaluation** and you can read more about it [here](https://codeburst.io/javascript-what-is-short-circuit-evaluation-ff22b2f5608c).

### Adding a contact

Let's start with adding a contact.

```javascript
function addContact() {
    if (!formIsValid()) {
        alert('oopsie :(')
        return
    }

    if (index = document.querySelector('#index').value) {
        editContact(index)
        return
    }

    const contacts = getAllContacts()
    contacts.push({
        firstName: document.querySelector('#firstname').value,
        lastName: document.querySelector('#lastname').value,
        phoneNumber: document.querySelector('#phonenumber').value
    })

    updateContacts(contacts)

    toggleFormVisibility()
    clearFormInputs()
}

document.querySelector('#add_new--button').addEventListener('click', addContact)
```
We are first using our input validating function to check if the user's input follows our guidelines and in case it isn't the function just stops.

The second if statement is used for the edit part and we'll get into that a little bit later.

After we have ensured the input is correct the user actually intends to add a contact we are retrieving the contact list and adding a new contact to it by pushing a new object with the values we got from the inputs.

In the end we update the contact list so it also includes the new contact and we hide the form and clear the inputs by using the functions we went over in the first sections.

### Updating the contacts

Near the end of the add contact function we have used the `updateContacts` function so let's see what that does.

```javascript
function updateContacts(contacts) {
    localStorage.setItem('contacts', JSON.stringify(contacts))

    renderContacts()
}
```

The function receives the contacts array as a parameter and takes that array and puts it into the local storage in a JSON format. You might have also notice above in the get all contacts function that we use `JSON.parse`. This is because local storage is limited and arrays cannot be put in there. To get over this limitation we have to turn our contacts array into JSON format.

After the contacts are updated in the local storage we go ahead and call the `renderContacts` function that will display the updated contacts in the HTML.

### Displaying the contacts

```javascript
function renderContacts() {
    const contactList = document.querySelector('#contacts--result')
    contactList.innerHTML = ''
    
    getAllContacts().forEach( (contact, index) => {
        const contactContainer = document.createElement('div')
        contactContainer.classList.add('contact--item')

        const name = document.createElement('h3')
        name.textContent = `${contact.firstName} ${contact.lastName}`
        contactContainer.appendChild(name)

        const phoneNumber = document.createElement('p')
        phoneNumber.textContent = contact.phoneNumber
        contactContainer.appendChild(phoneNumber)

        const editButton = document.createElement('button')
        editButton.textContent = 'Edit'
        editButton.classList.add('edit-button')
        editButton.dataset.id = index
        editButton.addEventListener('click', showEditForm)
        contactContainer.appendChild(editButton)

        const deleteButton = document.createElement('button')
        deleteButton.textContent = 'Delete'
        deleteButton.classList.add('delete-button')
        deleteButton.dataset.id = index
        deleteButton.addEventListener('click', deleteContact)
        contactContainer.appendChild(deleteButton)

        contactList.appendChild(contactContainer)
    })
}
```

Quite the chonker. Let's go over what it does.

We first need to select the container in which our contacts will be displayed and clear it.

What is `getAllContacts().forEach()` and that `=>` stuff supposed to mean??? Don't worry about it, let's turn it into a normal `for` loop.

```javascript
const contacts = getAllContact()
for (let index = 0; index < contacts.length; index++) {
  const contact = contacts[index]
  // rest of the code
}
```

Yes I used `index` instead of the classic `i`. Don't panic, this is so it doesn't mess up the rest of the code inside the `for`.

Let's talk about what happens in this `for` loop.

We first have to create a container to put all our contact information and buttons into and add the class of `contact--item` to it.

After that we have to create a new element for our name.

```javascript
name.textContent = `${contact.firstName} ${contact.lastName}`
```

Here we put put the first name and the last name together and then into the newly created name element. The equivalent of this is `name.textContent = contact.firstName + ' ' + contact.lastName`. The syntax I used above is called **template literals** and I strongly recommend to read more about it [here](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals).

Now that this name element is finished we have to attach it to the contact container by using `appendChild`.

The same process is applied for creating the phone number element so I'm not gonna get into it.

Lastly we have to add the edit and delete buttons.

Everything is pretty clear here, we create the buttons, put the text inside them, add classes and add event listeners so they actually do something. I'm going to go over the functions inside the event listeners after this section.

The weird part here is this `deleteButton.dataset.id = index`. By doing this we are attaching a reference to the button so we know what contact to delete when we press the button. To learn more about *data attributes** and the **dataset** i urge you to read [this](https://developer.mozilla.org/en-US/docs/Learn/HTML/Howto/Use_data_attributes).

### Removing a contact

```javascript
function deleteContact() {
    const contacts = getAllContacts()
    contacts.splice(this.dataset.id, 1)

    updateContacts(contacts)
}
```

All that we have to do here is get all the contacts, delete the contact at the index we get from the pressed button and then display the contacts again. If this seems confusing check the end of the previous section again.

### Editing a contact

```javascript
function showEditForm() {
    document.querySelector('#index').value = this.dataset.id

    const contact = getAllContacts()[this.dataset.id]

    document.querySelector('#add_new--button').textContent = 'Edit contact'

    document.querySelector('#firstname').value = contact.firstName
    document.querySelector('#lastname').value = contact.lastName
    document.querySelector('#phonenumber').value = contact.phoneNumber

    if (!document.querySelector('#form--container').classList.contains('active')) {
        toggleFormVisibility()
    }
}
```

When the edit button is pressed on a contact there is a hidden element inside the form that we have to fill in with that contact's id (index) so the next time we press the previously named add contact button it will edit the selected contact and not add a new one.

Confusing, I know. Let's see what the function does.

First, the hidden input is filled with the contact's id. 

After this we get the contact from our contacts array by using the id (which is also the index of the contact).

We have to change the text inside of the add contact button because this button will also be used to finish editing.

We also have to fill in all of the inputs with the information from the contact we just pressed edit on.

Last thing, when you click edit on a contact you need to toggle the form's visibility, but if the form is already open you don't want to do that so that is why the last `if` statement is there.

### Edit contact

```javascript
function addContact() {
    ...

    if (index = document.querySelector('#index').value) {
        editContact(index)
        return
    }
   
    ...
}
```

Remember this in the add contact form? Because we have set a value to that hidden input now the add contact function will lead to the edit one. Magical.

```javascript
function editContact(index) {
    const contacts = getAllContacts()
    contacts[index] = {
        firstName: document.querySelector('#firstname').value,
        lastName: document.querySelector('#lastname').value,
        phoneNumber: document.querySelector('#phonenumber').value
    }

    updateContacts(contacts)

    toggleFormVisibility()
    clearFormInputs()

    document.querySelector('#add_new--button').textContent = 'Add new contact'
}
```

We get the index as a parameter so all we have to do is get all the contacts and modify the contact at that specific index using the value from the form inputs.
After this, update the contacts, toggle the visibility of the form, clear the inputs and don't forget to change the text inside the button back.

### Displaying the contacts when we open the website

```javascript
document.addEventListener('DOMContentLoaded', renderContacts)
```

The `DOMContentLoaded` event is called your HTML is fully loaded and that is a great moment for us to display the contacts.

And we're done. All the code is tied up in the below section. Good job. I guess.

```javascript
function toggleFormVisibility() {
    document.querySelector('#form--container').classList.toggle('active')
}

function clearFormInputs() {
    document.querySelector('#firstname').value = ''
    document.querySelector('#lastname').value = ''
    document.querySelector('#phonenumber').value = ''
    document.querySelector('#index').value = ''
}

function formIsValid() {
    return /^[a-zA-Z]*$/.test(document.querySelector('#firstname').value) && 
        /^[a-zA-Z]*$/.test(document.querySelector('#lastname').value) &&
        /^[+]*[(]{0,1}[0-9]{1,4}[)]{0,1}[-\s\./0-9]*$/.test(document.querySelector('#phonenumber').value)
}

function getAllContacts() {
    return JSON.parse(localStorage.getItem('contacts')) || []
}

function renderContacts() {
    const contactList = document.querySelector('#contacts--result')
    contactList.innerHTML = ''
    
    getAllContacts().forEach( (contact, index) => {
        const contactContainer = document.createElement('div')
        contactContainer.classList.add('contact--item')

        const name = document.createElement('h3')
        name.textContent = `${contact.firstName} ${contact.lastName}`
        contactContainer.appendChild(name)

        const phoneNumber = document.createElement('p')
        phoneNumber.textContent = contact.phoneNumber
        contactContainer.appendChild(phoneNumber)

        const editButton = document.createElement('button')
        editButton.textContent = 'Edit'
        editButton.classList.add('edit-button')
        editButton.dataset.id = index
        editButton.addEventListener('click', showEditForm)
        contactContainer.appendChild(editButton)

        const deleteButton = document.createElement('button')
        deleteButton.textContent = 'Delete'
        deleteButton.classList.add('delete-button')
        deleteButton.dataset.id = index
        deleteButton.addEventListener('click', deleteContact)
        contactContainer.appendChild(deleteButton)

        contactList.appendChild(contactContainer)
    })
}

function updateContacts(contacts) {
    localStorage.setItem('contacts', JSON.stringify(contacts))

    renderContacts()
}

function addContact() {
    if (!formIsValid()) {
        alert('oopsie :(')
        return
    }

    if (index = document.querySelector('#index').value) {
        editContact(index)
        return
    }

    const contacts = getAllContacts()
    contacts.push({
        firstName: document.querySelector('#firstname').value,
        lastName: document.querySelector('#lastname').value,
        phoneNumber: document.querySelector('#phonenumber').value
    })

    updateContacts(contacts)

    toggleFormVisibility()
    clearFormInputs()
}

function deleteContact() {
    const contacts = getAllContacts()
    contacts.splice(this.dataset.id, 1)

    updateContacts(contacts)
}

function showEditForm() {
    document.querySelector('#index').value = this.dataset.id

    const contact = getAllContacts()[this.dataset.id]

    document.querySelector('#add_new--button').textContent = 'Edit contact'

    document.querySelector('#firstname').value = contact.firstName
    document.querySelector('#lastname').value = contact.lastName
    document.querySelector('#phonenumber').value = contact.phoneNumber

    if (!document.querySelector('#form--container').classList.contains('active')) {
        toggleFormVisibility()
    }
}

function editContact(index) {
    const contacts = getAllContacts()
    contacts[index] = {
        firstName: document.querySelector('#firstname').value,
        lastName: document.querySelector('#lastname').value,
        phoneNumber: document.querySelector('#phonenumber').value
    }

    updateContacts(contacts)

    toggleFormVisibility()
    clearFormInputs()

    document.querySelector('#add_new--button').textContent = 'Add new contact'
}

document.querySelector('#form_toggle').addEventListener('click', toggleFormVisibility)
document.querySelector('#add_new--button').addEventListener('click', addContact)
document.addEventListener('DOMContentLoaded', renderContacts)
```
