---
title: Lab 7
subtitle: Guessing Game
date: 2020-02-05T10:35:31.024Z
thumb_img_path: /images/J.J.-Abrams-explained-why-Baby-Yoda-is-the-heart-of.jpg
content_img_path: /images/J.J.-Abrams-explained-why-Baby-Yoda-is-the-heart-of.jpg
template: post
---
# HTML & CSS the smart way

You might be tempted to start writing these by yourself but there is no reason to do such a thing. There is a link to a working provided solution in the assignment document (http://nmd.redcapes.se/lab7/grade5/application.html) which can save us all of the work so we can go straight to the JavaScript part.

Once you have entered the link above, open up your DevTools and go to the Sources tab.

![](/images/Screenshot 2020-02-05 at 7.20.37 AM.png)

You can now go ahead and download the HTML and CSS files. The JavaScript file is obfuscated meaning it has been made unintelligible so don't try that.

*Everything written below is assuming you use these files.*

# JavaScript

I will not go over the requirements here since they can get confusing and at time they are not consistent with the solution provided. My recommendation is to go over them and then try to replicate the functionality found in the solution.

There are two main things we need to do, update the number of marbles shown when the user inputs a number and the actual guessing part.

## Updating the number of guessed marbles

So we need to update the number of marbles displayed every time the user types something into the input. Typing is an event so we can add an event listener to the input so it calls a function every time something is typed.

```javascript
document.querySelector('#user_guess').addEventListener('input', updateGuessedMarbles)
```

Of course we should also define the function.

```javascript
function updateGuessedMarbles() {
  // marble stuff
}
```

First, let's get the value from inside the input and we also need to check if that value is above 10 because in that case we don't want to update anything.

```javascript
const guess = document.querySelector('#user_guess').value

// exit the function if value is bigger than 10
if (guess > 10) {
 return  
}
```

After doing this, we will select the container in which we put the marbles and also make it empty so we don't keep continuously adding marbles to it.

```javascript
const resultsContainer = document.querySelector('#results_container')
resultsContainer.innerHTML = ''
```

Now that we have the container we can use a `for` starting at 1 and ending at the guessed number and a marble at each iteration.

```javascript
for (let i = 1; i <= guess; i++) {
  const marble = document.createElement('div')
  marble.setAttribute('class', 'marble')
  resultsContainer.appendChild(marble)
}
```

Let's put everything together now.

```javascript
function updateGuessedMarbles() {
  const guess = document.querySelector('#user_guess').value
  if (guess > 10) {
   return  
  }

  const resultsContainer = document.querySelector('#results_container')
  resultsContainer.innerHTML = ''
  
  for (let i = 1; i <= guess; i++) {
    const marble = document.createElement('div')
    marble.setAttribute('class', 'marble')
    resultsContainer.appendChild(marble)
  }
}

document.querySelector('#user_guess').addEventListener('input', updateGuessedMarbles)
```

## Guessing

Some HTML for an easier understanding:

```html
<form id="user_guess_form">
    <label for="">Make a guess between 1 - 10</label>
    <input id="user_guess" type="number">
    <button type="submit" id="user_guess_btn">Guess</button>
</form>
```

First of all we need to defined some variables to keep track of our score and remaining guesses.

```javascript
let score = 0
let guessesRemaining = 10
```

This time our event listener will be place on the whole form since submitting a form is an event. As you can see the Guess button has a type of submit meaning the form is submitted when clicked.

```javascript
document.querySelector('#user_guess_form').addEventListener('submit', makeGuess)
```

The definition of the function:

```javascript
function makeGuess(event) {
  // about 40 lines are about to go here so get ready
}
```

You probably noticed our function has an `event` parameter, this is because the function is part of an event listener. The default behaviour of forms is to go somewhere after you submit them but we don't want this, so this is where `event` comes into play. By using the `preventDefault` function we can avoid the basic form functionality.

```javascript
event.preventDefault()
```

We need to perform some checks before we begin with the actual guessing. The number of guesses remaining must be check and the value of the user's guess which needs to not be empty, smaller than 1 or bigger than 10. In case the proper conditions aren't met we exit the function.

```javascript
if (guessesRemaining < 1) {
  return
}

// making sure the value we get from the user is an actual number
const guess = Number(document.querySelector('#user_guess').value)

if (!guess || guess < 1 || guess > 10) {
  return
}
```

Let's generate the random number now.

```javascript
// formula: Math.floor(Math.random() * (max - min + 1)) + min
const randomNumber = Math.floor(Math.random() * 10) + 1
```

Now updating the score and the number of remaining guesses. To update the score we need to check if the user's guess is lower or equal than the random number we just generated. Also, the values on our page need to be updated as well.

```javascript
// update score if winner
if (guess <= randomNumber) {
  score += guess
}

guessesRemaining-- // minus one guess

// update values on page
document.querySelector('#number_guesses').innerText = guessesRemaining
document.querySelector('#score').innerText = score
```

With all of this done, let's proceed to add the marbles to the container, but we need to select it first and also clear it, just like in the updating function.

```javascript
const resultsContainer = document.querySelector('#results_container')
resultsContainer.innerHTML = ''
```

The question now is how many marbles are we adding to the container? And the answer to this question is that it depends.

![](/images/Screenshot 2020-02-05 at 11.04.27 AM.png)

In this case the user has won and we are displaying 5 marbles, 3 of them are green meaning the user's guess was 3 and 2 of them are red because the randomly generated number was 5. In this case the number of marbles is equals to the randomly generated number. Also, all of the marbles are filled meaning that the number of filled marbles is equal to the randomly generated number.

![](/images/Screenshot 2020-02-05 at 11.04.43 AM.png)

This is a losing scenario and we are displaying 9 marbles, 4 of them are filled meaning that the randomly generated number is 4 and the rest are blank because the guess was 9. In this case the number of marbles is equals to the user's guess.

We can now deduce that the number of marbles is equals to the greater number between the user's guess and the randomly generated number. Knowing this we can now determine the number of marbles and use a `for` loop to add them to our container.

```javascript
let numberOfMarbles;
if (randomNumber >= guess) {
  numberOfMarbles = randomNumber
}
else {
  numberOfMarbles = guess
}

for(let i = 0; i < numberOfMarbles.length; i++) {
  // fun stuff
}
```

Just like we did in the previous function, let's create a new marble.

```javascript
const marble = document.createElement('div')
marble.setAttribute('class', 'marble')
```

Knowing that marbles that are lower or equals to the random number are filled(check images above if confused) we can now add the background.

```javascript
if (i <= randomNumber) {
  marble.classList.add('background')
}
```

For the red and green borders, we need to check if the user has won or lost. If the guess is bigger than the generated number than all the marbles get a red border. If the guess is lower or equal as you can see in the first picture we can have both green and red borders. If the generated number is 5 and the guess is 3 we will only colour the first 3 marbles green and the last 2 red, meaning that we only colour the marbles that are lower or equals to the user's guess.

```javascript
if (guess > randomNumber) {
    marble.classList.add('loser')
}
else {
    if (i <= guess) {
        marble.classList.add('winner')
    }
    else {
        marble.classList.add('loser')
    }
}
```

The last thing we have to do is append the marble to the container.

```javascript
resultsContainer.appendChild(marble)
```

Now that we are done with the `for` loop the last thing this function does is empty the value of the input and focus it.

```javascript
document.querySelector('#user_guess').value = ''
document.querySelector('#user_guess').focus()
```

The whole thing looks like this:

```javascript
let score = 0
let guessesRemaining = 10

function makeGuess(event) {
  event.preventDefault()
  
  if (guessesRemaining < 1) {
    return
  }

  const guess = Number(document.querySelector('#user_guess').value)

  if (!guess || guess < 1 || guess > 10) {
    return
  }
  
  const randomNumber = Math.floor(Math.random() * 10) + 1
  
  if (guess <= randomNumber) {
    score += guess
  }
  guessesRemaining--
  document.querySelector('#number_guesses').innerText = guessesRemaining
  document.querySelector('#score').innerText = score
  
  const resultsContainer = document.querySelector('#results_container')
  resultsContainer.innerHTML = ''
  
  let numberOfMarbles;
  if (randomNumber >= guess) {
    numberOfMarbles = randomNumber
  }
  else {
    numberOfMarbles = guess
  }

  for(let i = 0; i < numberOfMarbles.length; i++) {
    const marble = document.createElement('div')
    marble.setAttribute('class', 'marble')
    
    if (i <= randomNumber) {
      marble.classList.add('background')
    }
    
    if (guess > randomNumber) {
      marble.classList.add('loser')
    }
    else {
      if (i <= guess) {
        marble.classList.add('winner')
      }
      else {
        marble.classList.add('loser')
      }
    }
    
    resultsContainer.appendChild(marble)
  }
  
  document.querySelector('#user_guess').value = ''
  document.querySelector('#user_guess').focus()
}
```

# The final result

```javascript
function updateGuessedMarbles() {
  const guess = document.querySelector('#user_guess').value
  if (guess > 10) {
   return  
  }

  const resultsContainer = document.querySelector('#results_container')
  resultsContainer.innerHTML = ''
  
  for (let i = 1; i <= guess; i++) {
    const marble = document.createElement('div')
    marble.setAttribute('class', 'marble')
    resultsContainer.appendChild(marble)
  }
}

document.querySelector('#user_guess').addEventListener('input', updateGuessedMarbles)

let score = 0
let guessesRemaining = 10

function makeGuess(event) {
  event.preventDefault()
  
  if (guessesRemaining < 1) {
    return
  }

  const guess = Number(document.querySelector('#user_guess').value)

  if (!guess || guess < 1 || guess > 10) {
    return
  }
  
  const randomNumber = Math.floor(Math.random() * 10) + 1
  
  if (guess <= randomNumber) {
    score += guess
  }
  guessesRemaining--
  document.querySelector('#number_guesses').innerText = guessesRemaining
  document.querySelector('#score').innerText = score
  
  const resultsContainer = document.querySelector('#results_container')
  resultsContainer.innerHTML = ''
  
  let numberOfMarbles;
  if (randomNumber >= guess) {
    numberOfMarbles = randomNumber
  }
  else {
    numberOfMarbles = guess
  }

  for(let i = 0; i < numberOfMarbles.length; i++) {
    const marble = document.createElement('div')
    marble.setAttribute('class', 'marble')
    
    if (i <= randomNumber) {
      marble.classList.add('background')
    }
    
    if (guess > randomNumber) {
      marble.classList.add('loser')
    }
    else {
      if (i <= guess) {
        marble.classList.add('winner')
      }
      else {
        marble.classList.add('loser')
      }
    }
    
    resultsContainer.appendChild(marble)
  }
  
  document.querySelector('#user_guess').value = ''
  document.querySelector('#user_guess').focus()
}

document.querySelector('#user_guess_form').addEventListener('submit', makeGuess)


```
