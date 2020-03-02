---
title: Lab 8
subtitle: The bane of my existence.
date: 2020-02-18T20:53:21.963Z
thumb_img_path: /images/madboi.jpg
content_img_path: /images/madboi.jpg
excerpt: The bane of my existence.
template: post
---
## First off, this assignment promotes a very bad and unintuitive use case for local storage and there is nothing to learn out of it. All this lab can do for you is manufacture confusion and frustration mostly because even the example websites in the PDF are very buggy.

#### *The guide will be added soon&trade;.*

[Custom foo description](#foo)

### Foo

Check the lab 7 post to see how to get the html and css files.

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
