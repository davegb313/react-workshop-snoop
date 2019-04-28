<a name="thatop"></a>
# forms & user input in react: snoop jobs

Snoop wants us to build him a job application form, and he wants it to be **fresh**

Agenda:

- [step 1: on the button](#step1)
- [step 2: give'm what they want - text input](#step2)
- [step 3: break'm off somethin - drop downs](#step3)
- [step 4: and it's gotta be bumpin - refactor to components](#step4)
- [step 5: city of compton - notifications and animations](#step5)


## getting started

`$ create-react-app snoop-jobs`

`$ cd snoop-jobs`

`$ npm start`

you now have the default create-react-app starter running in your browser and can edit the `src` files live

you are now also ready to work through the [exercises](/exercises.md)!

you should be fairly comfortable with `setState` `onClick` and `onChange` before starting. If you aren't yet, go do the exercises first!



<a name="step1"></a>
## step 1: on the button

<img src='http://3.bp.blogspot.com/-wojpCbgYsz8/UB9hbJnCo_I/AAAAAAAABlk/GHXo4H9vFck/s1600/Snoop-Doggy-Dogg-24.jpg' height=250 width=325/>

Uncle Snoop wants to make sure we can get events from the user. First thing's first - we need a `<button>` for them to press. You might say it's a bit backwards to start with a done button, but you know how Snoop likes to flip the script.


<sub>./src/App.js</sub>
```js
import React, { Component } from 'react';
import './App.css';

class App extends Component {
  render(){
    return (
      <div className='App'>
        <button> Done </button>
      </div>
    );
  }
}

export default App;
```

and let's give it an instance method to call, using a fat arrow to log out a message `'done'` (just to test that our button works)

<img src='' width=1 height=200/>

<sub>./src/App.js</sub>
```js
import React, { Component } from 'react';
import './App.css';

class App extends Component {

  done = ()=> console.log('done')

  render(){
    return (
      <div className='App'>
        <button onClick={this.done}> Done </button>
      </div>
    );
  }
}

export default App;
```

`done = ()=> console.log('done')` is defining `this.done` to be a function (we're using the [fat arrow](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions) in one line who simply calls `console.log` with a string`'done'`

notice that when we bind the instance method to the button with `onClick={this.done}` we need to say `this.done` but when we defined it it was just `done = ()=> ...`

what we'll be doing in this workshop is learning to get user values into `state`, so let's init a state and log it out in `this.done`

remember that `state = {}` goes at the top of our `class` (that way we always know where to find it)

<sub>./src/App.js</sub>
```js
import React, { Component } from 'react';
import './App.css';

class App extends Component {

  state = {
    rapName: 'Nate Dogg z"l',
    albumSales: 4200000,
  }

  done = ()=> console.log('done', this.state)

  render(){
    return (
      <div className='App'>
        <button onClick={this.done}> Done </button>
      </div>
    );
  }
}

export default App;
```

---

[back to the top](#thatop)

---


<a name="step2"></a>
## step 2: give'm what they want - text input

### editing the rap name

we have a `rapName` listed in our `state`, now it's time to let the user update it

first we'll render an `<input />` element with the value in it

<sub>./src/App.js</sub>
```js
//...

  render(){
    return (
      <div className='App'>
        <input value={this.state.rapName}/>
        <button onClick={this.done}> Done </button>
      </div>
    );
  }

//...
```

so that's great if the value never changes, but Snoop wants the user's input to be set in the `.state` so it can be rendered back to the user!

we learned already that everything that changes that the users see has to be from the state.

So every time the user types even one character, we need to set that in the state - only then it can render to the user.


To take text input from a user, we're going to put an `onChange` eventHandler function on our `<input />` which sets the new value in `state` using `setState`

This pattern is standard in React and goes by the name: [controlled input pattern](https://reactjs.org/docs/forms.html)

<sub>./src/App.js</sub>
```js
//...
  
  setRapName = event => this.setState({ rapName: event.target.value })

  render(){
    return (
      <div className='App'>
        <input value={this.state.rapName} onChange={this.setRapName}/>
        <button onClick={this.done}> Done </button>
      </div>
    );
  }

//...
```

### card container for form input

Snoop wants his form elements to be stylish as well as functional

Let's make a card (like in [material design](https://material-ui.com/demos/cards/))

first we'll wrap our `<input />` with a container (.card) `<div>...</div>`

<sub>./src/App.js</sub>
```html
//...
  render(){
    return (
      <div className='App'>
        <div className="card">
          <input value={this.state.rapName} onChange={this.setRapName}/>
        </div>
        <button onClick={this.done}> Done </button>
      </div>
    );
  }

//...
```

and style `.card` to be the size we want (using min-* and max-* height/width is a responsive best practice)


<sub>./src/App.css</sub>
```css
//...

.card {
  min-width: 300px;
  min-height: 80px;

  margin: 10px;
}
```

and we can add a bit of `box-shadow` to give the card a bit of pop

<sub>./src/App.css</sub>
```css
//...

.card {
  min-width: 300px;
  min-height: 80px;

  margin: 10px;
  
  box-shadow:
  0px 1px 3px 0px rgba(0,0,0,0.2),
  0px 1px 1px 0px rgba(0,0,0,0.14),
  0px 2px 1px -1px rgba(0,0,0,0.12);
}
```

we'll be reusing this `.card` container for the rest of our form elements!


### floating label

Now we need to make it clear to the user what this input is for!

let's add a `<label>...</label>` around it (wrapping with `<label/>` means the user clicking on the label focuses the `<input/>`)


<sub>./src/App.js</sub>
```html
//...
  render(){
    return (
      <div className='App'>
        <div className="card">
          <label>
            Rap Name
            <input value={this.state.rapName} onChange={this.setRapName}/>
          </label>
        </div>
        <button onClick={this.done}> Done </button>
      </div>
    );
  }

//...
```

Snoop wants it [swanky](https://material-ui.com/demos/text-fields/)


#### position: absolute;

to accomplish this, we'll need to have a [position: relative](https://www.google.com/search?q=position+relative) container `<div/>` and [position: absolute](https://www.google.com/search?q=position+absolute) children

<sub>./src/App.js</sub>
```html
//...
  render(){
    return (
      <div className='App'>
        <div className="card swanky-input-container">
          <label>
            <input value={this.state.rapName} onChange={this.setRapName}/>
            <span className='title'>Rap Name</span>
          </label>
        </div>
        <button onClick={this.done}> Done </button>
      </div>
    );
  }

//...
```

we'll see later why the label text moved into a `<span>` and why it comes after the `<input/>`

[or you can read ahead on sibling selectors and see if you can figure it out yourself](https://developer.mozilla.org/en-US/docs/Web/CSS/Adjacent_sibling_combinator)


#### flat input

we'll style the `<input/>` first, then move on to the label text

<sub>./src/App.css</sub>
```css
//...

.swanky-input-container {
  position: relative;
}

.swanky-input-container input {
  position: absolute;
  
  bottom: 2px;
  left: 2px;
}

```

now we can get rid of the border and set the size

<sub>./src/App.css</sub>
```css
//...

.swanky-input-container {
  position: relative;
}

.swanky-input-container input {
  position: absolute;
  border: none;
  
  bottom: 2px;
  left: 2px;
  width: 100%;
  height: 60px;

  font-size: 22px;
  padding-left: 10px;

}

```

!!! oh no, our input is too wide... why is that?

open up the computed styles panel (right click the input -> Inspect Element... Computed panel) to review

we can see that we have 2px of position on the left, 10px of padding on the left (we put that in so our text would look good, but it adds to the total box width)

so we want our `<input />` to be 14px (2px left + 2px right + 10px padding) less than 100%

luckily, CSS3 added the [calc function](https://developer.mozilla.org/en-US/docs/Web/CSS/calc) for this exact case

where we had

```css
  width: 100%;
```

we can put (be careful to put the spaces around the `-`)

```css
  width: calc(100% - 14px);
```

now let's add an underline like in the design spec

<sub>./src/App.css</sub>
```css
//...

.swanky-input-container input {
  position: absolute;
  border: none;
  border-bottom: 1px solid black;
  //...
  
```

#### :focus, transition

when the input is `:focus`ed (the user is typing in it), we want the underline to be green and that pesky outline to go away.

we'll use the [:focus pseudoselector](https://www.google.com/search?q=focus+pseudoselector) to style the input while it is `:focus`ed

<sub>./src/App.css</sub>
```css
//...

.swanky-input-container input:focus {
  outline: none;
  font-size: 26px;
  border-bottom: 2px solid green;
}
```

I've also increased the size of the text.

last thing, we want to apply a [css transition](https://www.w3schools.com/css/css3_transitions.asp) to the font-size

<sub>./src/App.css</sub>
```css
//...

.swanky-input-container input {
  position: absolute;
  border: none;
  border-bottom: 1px solid black;
  background-color: white;
  
  bottom: 2px;
  left: 2px;
  width: calc(100% - 14px);
  height: 60px;

  font-size: 22px;
  padding-left: 10px;

  transition: font-size 0.25s;
}

//...
```

great, now we can style the label



#### absolute label

now we can position the label properly (in the top left of the container)

<sub>./src/App.css</sub>
```css
//...

.swanky-input-container span.title {
  position: absolute;
  top: 5px;
  left: 5px;
  cursor: pointer;
  
  font-weight: 300;
  font-size: 16px;
}
```

I've also updated the `cursor` so it appears clickable to the user

and set the font how I think looks good.


### label transitions on input:focus

now we'll see why we had to put the label text in a `<span>` after the `<input/>`

we want to change the `font-size` and `color` of the label text when the `<input/>` is `:focus`ed

the way we'll do that is by making a selector:

```
select the .swanky-input-container class's child
which is a span with className title
which comes AFTER a :focused input
```

which looks like

<sub>./src/App.css</sub>
```css
.swanky-input-container input:focus + span.title {
  color: green;
  font-size: 12px;
}
```

since there's no `PREVIOUS sibling` selector in CSS, we had to put the `<span>` after the `<input/>` to be able to select based on its `:focus`

[for practice with complex css selectors, play this game](https://flukeout.github.io/)


now we can put a transition on both of those styles and everything should be solid

<sub>./src/App.css</sub>
```css
//...

.swanky-input-container span.title {
  position: absolute;
  top: 5px;
  left: 5px;
  cursor: pointer;
  
  font-weight: 300;
  font-size: 16px;
  
  transition: font-size 0.25s, color 1s;
}

//...
```

that's pretty swanky


### email, validation

Snoop wants to be able to contact his applicants if they throw down lots of fresh work like that input we just made.

So let's make another card right after the first for an email address

<sub>./src/App.js</sub>
```html
//...
  <div className="card swanky-input-container">
    <input value={this.state.email} onChange={this.setEmail} />
    <span className='title'>Email</span>
  </div>
//...
```

we'll also want to initialize `state.email` and make a setter method

```js
//...
  
  state = {
    rapName: 'Nate Dogg z"l',
    albumSales: 4200000,
    email: '',
  }
  
  //...
  
  setEmail = event => this.setState({ email: event.target.value })
  
  //...
```

bam! right away that's looking good.

Hold up a minute tho - Uncle Snoop has seen some skeezy email addresses in his day - he's gonna want us to validate.


#### controlled input validation

The controlled input pattern gives us a very straightforward way to build a validation UX.

Every time our value changes, we have out setter instance method being called... so to tell the user if their input is valid, we should compute a boolean `isEmailInvalid` in that same function, save it to the `state` and use it in `render` a validation failure message

let's see what that looks like


<sub>./src/App.js</sub>
```js
//...
  state = {
    rapName: 'Nate Dogg z"l',
    albumSales: 4200000,
    email: '',
    isEmailInvalid: false,
  }
  
  //...
  
  setEmail = event => this.setState({
    email: event.target.value,
    isEmailInvalid: !event.target.value.includes('@gmail.com'),
  })
  
  //...
```

we've now just added one more value in `state`

it starts out as `false` (if the user hasn't entered anything, the email van't be invalid yet)

then as soon as the user types, we check whether each new value contains the string `@gmail.com` to determine if it is a valid email

if you use hotmail still, it just won't work!



#### rendering from validation state

if the email is inValid, we want to render out an extra `<span>` with our validation message


```html
//...
  <div className="card swanky-input-container">
    <label>
      <input value={this.state.email} onChange={this.setEmail} />
      <span className='title'>Email</span>
      {this.state.isEmailInvalid ? (
        <span className="invalid">Please enter a valid email address</span>
      ) : null}
    </label>
  </div>
//...
```

so far so good, now we can style it to fit in the top right corner of our card


<sub>./src/App.css</sub>
```css
//...

.swanky-input-container span.invalid {
  position: absolute;
  top: 5px;
  right: 5px;

  font-size: 12px;
  color: red;
  text-align: right;
}

```

that's great and all, but maybe it shouldn't show up while we're typing!

let's use the same trick as last time, but now with the [general sibling selector](https://developer.mozilla.org/en-US/docs/Web/CSS/General_sibling_combinator) to `display: none;` the invalidation message while the `input` is `:focus`

and the [:not() pseudoselector](https://developer.mozilla.org/en-US/docs/Web/CSS/:not) to style it otherwise

<sub>./src/App.css</sub>
```css
//...

.swanky-input-container input:focus ~ span.invalid {
  display: none;
}

.swanky-input-container input:not(:focus) ~ span.invalid {
  position: absolute;
  top: 5px;
  right: 5px;

  font-size: 12px;
  color: red;
  text-align: right;
}
```

Fresh!


### email validation regex

Our "gmail only" rule is a bit harsh! Snoop wants us to use the RFC 5322 Official Standard [email regex](https://emailregex.com/) validator to mellow out the vibe.

To accomplish this goal, we'll need to learn how to [check a string with a regex in javascript](https://www.google.com/search?q=check+string+regex+js)

now that we've done that, let's plop that regex down into a utility file, export it, import it to App.js and use it for our check


`$ touch ./src/emailRegex.js`

<sub>./src/emailRegex.js</sub>
```js
// eslint-disable-next-line
export default /(?:[a-z0-9!#$%&'*+/=?^_`{|}~-]+(?:\.[a-z0-9!#$%&'*+/=?^_`{|}~-]+)*|"(?:[\x01-\x08\x0b\x0c\x0e-\x1f\x21\x23-\x5b\x5d-\x7f]|\\[\x01-\x09\x0b\x0c\x0e-\x7f])*")@(?:(?:[a-z0-9](?:[a-z0-9-]*[a-z0-9])?\.)+[a-z0-9](?:[a-z0-9-]*[a-z0-9])?|\[(?:(?:25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.){3}(?:25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?|[a-z0-9-]*[a-z0-9]:(?:[\x01-\x08\x0b\x0c\x0e-\x1f\x21-\x5a\x53-\x7f]|\\[\x01-\x09\x0b\x0c\x0e-\x7f])+)\])/;
```

<sub>./src/App.js</sub>
```js
import React, { Component } from 'react';
import './App.css';

import emailRegex from './emailRegex';

class App extends Component {
  //...
  
  setEmail = event => this.setState({
    email: event.target.value,
    isEmailInvalid: !emailRegex.test( event.target.value ),
  })
  
  //...

```

that's way smoother now.


### albums sales, number format

Aight. Snoop's gonna need to know how many albums you've been slangin

like before, we'll need a setter instance method

<sub>./src/App.js</sub>
```js
  setAlbumSales = event => this.setState({ albumSales: event.target.value })
```

and a JSX `<input />` tag with `value` and `onChange` bindings, along with our `<label>...<span>` solution from the last section

```html
          <div className="card swanky-input-container">
            <label>
              <input value={this.state.albumSales}
                     type='number'
                     step={1000}
                     onChange={this.setAlbumSales} />
              <span className='title'>Album Sales</span>
            </label>
          </div>

```

here, I've set `type='number'` and `step={1000}` to take advantage of the built in HTML number input behaviour.


### gold and platinum albums

Snoop is very impressed with your number input, he just isn't connecting to it. He wants us to display gold records on top of the input for every million albums sold (up to four).

[he's taken a picture of one of his gold records and made a transparent background for us](https://github.com/nikfrank/react-course-workbook-2/blob/step7/src/goldRecord.png?raw=true)


first, let's render out all four albums, then we can filter that down

<sub>./src/App.js</sub>
```js
//... make sure to download the file to the same directory!

import goldRecord from './goldRecord.png';

//...
```

```html
          <div className="card swanky-input-container">
            <label>
              <input value={this.state.albumSales}
                     type='number'
                     step={1000}
                     onChange={this.setAlbumSales} />
              <span className='title'>Album Sales</span>
              <div className='goldRecords'>
                {
                  [1,2,3,4]
                    .map(n => (
                      <div className='goldRecord' key={n} >
                        <img src={goldRecord} alt='solid gold'/>
                      </div>
                    ))
                }
              </div>
            </label>
          </div>

```

we'll use the outer `<div>` as a flex container (going right to left) starting from 50px from the right side of the swanky-input-container

<sub>./src/App.css</sub>
```css
//...

.goldRecords {
  position: absolute;
  right: 50px;
  height: 100%;
  
  display: flex;
  flex-direction: row-reverse;
  align-items: center;
}
```

and we'll use the extra `<div>` wrapping the `<img/>` to make the records overlap

```css
//...

.goldRecord {
  width: 20px;
}

.goldRecord img {
  width: 50px;
  height: 100%;
}
```

now let's use our skills with [Array.filter](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/filter) to show the right number of records

<sub>./src/App.js</sub>
```html
          <div className="card swanky-input-container">
            <label>
              <input value={this.state.albumSales}
                     type='number'
                     step={1000}
                     onChange={this.setAlbumSales} />
              <span className='title'>Album Sales</span>
              <div className='goldRecords'>
                {
                  [1,2,3,4]
                    .filter(n => n*1000000 <= this.state.albumSales)
                    .map(n => (
                      <div className='goldRecord' key={n} >
                        <img src={goldRecord} alt='solid gold'/>
                      </div>
                    ))
                }
              </div>
            </label>
          </div>
```

solid!

We're using a common pattern in React here of computing multiple things from one `state` value.

Here we could compute the number of albums in the `setAlbumsSales` method (and in some cases we'll want to do that, eg: for efficiency), here though it's easy enough to use `state.albumSales` to compute out a list of `<img/>`s

if the computation is more complex (sorting, arithmetic, etc), we would consider not doing it inside the render function, as that would slow down our app.


<a name="step3"></a>
## break'm off somethin - drop downs

Snoop is going to want to know more than a name and a number!

<img src='http://hiphopgoldenage.com/wp-content/uploads/2015/07/42-27797293.jpg' height=250 width=325/>

He also has a list of open positions to apply to, and is going to want to know where you're from (country / state)

first let's initialize some values in `state`

<sub>./src/App.js</sub>
```js
//...

  state = {
    rapName: 'Nate Dogg z"l',
    albumSales: 4200000,
    email: '',
    isEmailInvalid: false,
    job: '',
    country: '',
    topAlbum: null,
    topRapper: '',
    startDate: null,
  }
```


### pick a job

<sub>./src/App.js</sub>
```js
  setJob = event => this.setState({ job: event.target.value })
```

```html
          <div className="card swanky-input-container">
            <label>
              <select onChange={this.setJob} value={this.state.job}>
                <option value=''>Select Job</option>
                <option value='rapper'>rapper</option>
                <option value='sales'>sales</option>
                <option value='distribution'>distribution</option>
              </select>
              <span className='title'>Job</span>
            </label>
          </div>

```

here we'll also want to add selectors for the select, and to fix the `font-size` for the `<option>` tags (to reduce jank)

<sub>./src/App.css</sub>
```css
//...

.swanky-input-container select,
.swanky-input-container input {

//...

.swanky-input-container select:focus,
.swanky-input-container input:focus{

//...

.swanky-input-container select:focus + span.title,
.swanky-input-container input:focus + span.title {


//...

option {
  font-size: 22px;
}
```

### dropdown with an image (album selection)

Snoop is all about the music. So when someone applies for a job with him, he's going to want to know that person's favourite rap album

We could make "just another dropdown", but we'd rather really impress Snoop by making a dropdown that also shows the album cover `<img/>`!

unfortunately, when we try to render an `<img/>` into an `<option/>` we get an error `Only strings and numbers are supported as <option> children.`

So we're going to have to build the entire dropdown from scratch using `<div/>`s

#### downloading assets

first, let's decide on a list to choose from

|name|year|cover|
|---|---|---|
|Doggystyle|1993|<img src="https://upload.wikimedia.org/wikipedia/en/6/63/SnoopDoggyDoggDoggystyle.jpg" height=200 width=200/>|
|Tha Doggfather|1996|<img src="https://upload.wikimedia.org/wikipedia/en/a/a3/Tha-doggfather.jpg" height=200 width=200/>|
|Da Game Is to Be Sold, Not to Be Told|1998|<img src="https://upload.wikimedia.org/wikipedia/en/c/c5/Gameistobesold.jpg" height=200 width=200/>|
|No Limit Top Dogg|1999|<img src="https://upload.wikimedia.org/wikipedia/en/d/d1/Snoop_front.JPG" height=200 width=200/>|
|Tha Last Meal|2000|<img src="https://upload.wikimedia.org/wikipedia/en/d/dc/The_Last_Meal_-_Front.jpeg" height=200 width=200/>|

Obviously everything Snoop puts out is fresh, so we can always add more later.

You can go ahead and grab the album cover image source urls from here


#### importing assets

let's make an index file for all the albums to keep things organized

`$ touch ./src/snoopAlbums.js`

<sub>./src/snoopAlbums.js</sub>
```js
import doggystyle from './doggystle.jpg';

export default [
  { name: 'Doggystyle', year: 1993, cover: 'https://upload.wikimedia.org/wikipedia/en/6/63/SnoopDoggyDoggDoggystyle.jpg' },
];
```

we `export` an array of albums, each of which is an object

each album's object will need a field for `name` string `year` number and `cover` image

(I'll leave it as an exercise to follow this pattern and complete the list of albums)


#### rendering the selection


let's render out the two possible closed states (no selection made, selection already made)

<sub>./src/App.js</sub>
```html
//...

          <div className="card swanky-input-container">
            <span className='title'>Top Album</span>
            <div className="album-dropdown-base">
              {this.state.topAlbum === null ? (
                 <span>Select Top Album</span>
              ):(
                 <>
                   <img src={this.state.topAlbum.cover} alt={this.state.topAlbum.name}/>
                   <span>{this.state.topAlbum.year}</span>
                   <span>{this.state.topAlbum.name}</span>
                 </>
              )}
              <span className='drop-arrow'>▼</span>
            </div>
          </div>
//...
```

<sub>./src/App.css</sub>
```css
//...


.album-dropdown-base {
  display: flex;
  align-items: center;
  justify-content: space-between;
  cursor: pointer;

  padding: 30px 15px 10px 10px;
}

.album-dropdown-base img {
  height: 45px;
  margin-top: -5px;
  width: auto;
}

.album-dropdown-base .drop-arrow {
  position: absolute;
  right: 5px;
  user-select: none;
}
```

now if we set `topAlbum: snoopAlbums[0]` in our `state = {...}` initialization, we'll see doggystyle render out


#### toggling the dropdown open state

now that the closed states render correctly, we need to give our dropdown the ability to open

<sub>./src/App.js</sub>
```js
  state = {
    //...
    topAlbumOpen: false,
  }

  toggleTopAlbumOpen = ()=> this.setState(state => ({ topAlbumOpen: !state.topAlbumOpen }))

  //...
```


now that we can toggle the state, we have three things to render

1. switch the drop arrow to point up
2. the selectable items
3. an invisible "click out" div behind the selectable items, so when the user clicks out, we can close the dropdown




#### switch the drop arrow to point up

```html
  <span className='drop-arrow'>{this.state.topAlbumOpen ? '▲' : '▼'}</span>
```

that was easy!



#### the selectable items

we're going to render a `<ul>` when the `topAlbumOpen` is `true`

<sub>./src/App.js</sub>
```js
          <div className="card swanky-input-container">
            <span className='title'>Top Album</span>
            <div className="album-dropdown-base" onClick={this.toggleTopAlbumOpen}>
              {this.state.topAlbum === null ? (
                 <span>Select Top Album</span>
              ):(
                 <>
                   <img src={this.state.topAlbum.cover} alt={this.state.topAlbum.name}/>
                   <span>{this.state.topAlbum.year}</span>
                   <span>{this.state.topAlbum.name}</span>
                 </>
              )}
              <span className='drop-arrow'>{this.state.topAlbumOpen ? '▲' : '▼'}</span>
            </div>
            {!this.state.topAlbumOpen ? null : (
               <ul className='selectable-albums'>
               </ul>
            )}
          </div>
```

and in it, we'll render a list of the albums

```js
       <ul className='selectable-albums'>
         {snoopAlbums.map(({ name, year, cover })=> (
           <li key={name} onClick={()=> this.selectAlbum({ name, year, cover })}>
             <img src={cover} alt={name}/>
             <span>{year}</span>
             <span>{name}</span>
           </li>
         ))}
       </ul>
```

and of course, an instance method to select the album

```js

  selectAlbum = topAlbum => this.setState({ topAlbum, topAlbumOpen: false })
  
```

which also closes the dropdown!


now let's style it to work

<sub>./src/App.css</sub>
```css
.album-dropdown-base,
ul.selectable-albums li {
  display: flex;
  align-items: center;
  justify-content: space-between;
  cursor: pointer;
  padding: 10px;
}


.album-dropdown-base {
  padding-top: 30px;
  padding-right: 15px;
}

ul.selectable-albums li:hover {
  background-color: #eee;
}

.album-dropdown-base img,
ul.selectable-albums img {
  height: 45px;
  margin-top: -5px;
  width: 45px;
}

.album-dropdown-base span:not(.drop-arrow),
ul.selectable-albums li span {
  display: inline-block;
  width: 180px;
  text-overflow: ellipsis;
  white-space: nowrap;
  overflow: hidden;
}

.album-dropdown-base span:not(.drop-arrow):nth-child(2),
ul.selectable-albums li span:nth-child(2) {
  width: 50px;
}


.album-dropdown-base .drop-arrow {
  position: absolute;
  right: 5px;
  user-select: none;
}

ul.selectable-albums {
  list-style: none;
  padding: 0;

  position: absolute;
  top: 100%;

  max-height: 25vh;
  overflow-y: auto;
  
  margin: 2px 0 0 0;

  background-color: white;
  box-shadow:
    0px 1px 3px 0px rgba(0,0,0,0.2),
    0px 1px 1px 0px rgba(0,0,0,0.14),
    0px 2px 1px -1px rgba(0,0,0,0.12);
  
}
```


#### the click out

<sub>./src/App.js</sub>
```js
    {!this.state.topAlbumOpen ? null : (
       <>
         <div className='click-out' onClick={this.clickOut}/>
         <ul className='selectable-albums'>
           {snoopAlbums.map(({ name, year, cover })=> (
             <li key={name} onClick={()=> this.selectAlbum({ name, year, cover })}>
               <img src={cover} alt={name}/>
               <span>{year}</span>
               <span>{name}</span>
             </li>
           ))}
         </ul>
       </>
    )}
```

here we're using the newest syntax for `React.Fragment`, which is a fake element which just wraps a bunch of tags so that there's technically one root element returned from an expression (here we need it because of the conditional ternary operator)

and the instance method

```js
  clickOut = ()=> this.setState({ topAlbumOpen: false })
```

and the styles we need to capture the clicks when they aren't on the dropdown list items

<sub>./src/App.cs</sub>
```css
//...

.click-out {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
}
```


#### bonus: styling the scrollbar

Snoop is probably going to think that scrollbar looks a bit 1992, and not in the good way.

Let's be proactive and funk it up

[styling scrollbars is fairly new in CSS, so I find CSS tricks to be a great resource for this kind of thing](https://css-tricks.com/almanac/properties/s/scrollbar/)

```css
.album-dropdown-base {
  padding-top: 30px;
  padding-right: 15px;
}

ul.selectable-albums::-webkit-scrollbar {
  width: 5px;
}

ul.selectable-albums::-webkit-scrollbar-track {
  background-color: #0000;
}

ul.selectable-albums::-webkit-scrollbar-thumb {
  background-color: #7d79;
  border-radius: 2.5px;
}


```


### menu hover dropdown -> header
### autocomplete / filter dropdown (country)
### picking a date


<a name="step4"></a>
## and it's gotta be bumpin - refactor to components

### snoop SVG, albums
### floating label input
### image dropdown
### autocomplete dropdown


<a name="step5"></a>
## city of compton - notifications and animations

### toastr on submit
### transition on cards entry
### transition on floating labels
### transition on validation colors
### transition on dropdowns
### tupac ak-47!!!!


This project was bootstrapped with [Create React App](https://github.com/facebook/create-react-app).

