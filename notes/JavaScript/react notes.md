\*\*learn React JS with Tino

## first of all to setup a project:\*\*

1. _npm create vite@latest_
1. _2\. do the selections_

_3. npm install //this one it can just run auto_

##### **basic syntax:**

react uses components, reusable piece of code you can use anywhere.

the basic form is

function Header (){

&nbsp;here you declare jx functions that you want to use inside the Jawn.

return(

&nbsp; <>

<p>here you return html elements, but the catch is it only returns a single element that's why the angle brackets, or simply return a <div> which contain you code and the children, sooo if its html your retuning here we all know its just bones no style no brain, to style you name it classname=give a classname, we don't use class since its a reserved word, then to give it brain JavaScript!! use {inject your js in here}, yae that's all you need to know. go wild</p>

&nbsp; </>

)}

export default header; // so that you can be able to import this component and use it anywhere.

##### **props:**

short for properties, this is passing properties to our components so that we can pass different values when we use the component at different times

syntax:

function Student(props){

return(

&nbsp;<div>

<p>student name: {props.name}</p>

<p>student age: {props.age}</p>

<p>student gender: {props.gender}</p>

</div>

)};

export default Student;

what then happen is when you import Student somewhere, you have to pass in the properties ant they will work just fine

propTypes allows us to make sure that the passed data/value is of the correct datatype

and obviously you have to import proptypes from 'prop-types'

Student.propType = {

name: propTypes.string

age :propTypes.number

gender :proptypes.string

}

lastly you have to setup default props which are the go to value if nothing is passed

Student.DefaultProps = {

name: "guest"

age: 0

gender: "male"

};

##### **conditional rendering**

make use of props to check the passed data, use it to make a decision on what to display example:
function Login(props){
return(

props.isLoggedIn ? <h1>welcome{props.username}</h1> : <h1>please loggin to continue </h1>

// also the ternary operator, simple and clean way to use conditionals as compared to tradtional if - else

)};

export default Login;

##### **click events**

an interaction when a user clicks a specific element and respond by passing a call back function on the on click event handler

- create a component
- create function to be executed on click
- use the onClick/onDoubleClick event handler inside the component
- call the function

example:

function Button(){

const handleclick()=>(){

console.log("hello")

}

return(

<Button onClick{handleclick}>clich me</Button>

// use arrow func when passing props, because using handleclick(props)will call the func even without clicking it

)

};

export default Button

event handler parameter(e) allow us to access different properties

function Button(){

const handleclick(e)=> e.target.text = "hey"

//you can access a lot of properties using e.

return(

<Button onClick{(e) =>(e)}>click me</Button>

)}

##### **React hooks**

these are special functions that allow functional components to use react features without writtiing class components

these are useState, useEffect, useContext, useReducer,useCallback and many

**useState(**)- allows the creation of a stateful variable and a setter func to update its value in the virtual dom

**example:**

import { useState } from 'react'; // import it

function MyComponent{

const [name, setName] = useState("initial value") //set an array of variable, the orig and the other to update orig, it can have an initial value

const updateName = ()=>{

setName(chris)} //use the setName to update the value of the name variable in the virtual dom

return(

<Button onClick{updateName}>update name</Button>

)

};

_todo: create a counter program to increase number and decrease the value onclick_ ❗❗

#### **useEffect():**

used to run a piece of side code when something happens

useEffect(() =>{}) - runs after every re render

useEffect(() =>{},\[]) - runs only on mount

useEffect(() =>{},\[value]) - runs on mount = when value changes

useEffect(() =>{

side code

});

uses:

event listeners

DOM manipulation

real time updates

fetching data from api

clean up when a component un mount

##### **OnChange event handler**

this is an event handler used in from element like, input, text, option

it triggas a function everytime the value changes in the input

example:

function MyComponent(){

const \[name, setName] = useState("")

function handleInputchange(e){

setName(e.target.value)

}

return(

<>

<input onChange = {handleInputChange} value = {name} />

<p>name: {name}</p>

</>

)

};

export default MyComponent

###### **updater functions**

used when passing the value in the setter

setCount(count + 1)

if we are to increase value 3 times doing this would not work

setCount(count + 1)

setCount(count + 1)

setCount(count + 1)

because it work keep the prev state it just take the initial state and update it, that why updater functions are more prefferable

it works like this

setCount(c => c + 1)

use the arrow function, this allows to keep the prev state so that if you repeat 3 times it will move 3 times by keeping track of the prev state

//naming convention is prevCount or just take the first letter of the variable name in this case c, like how i used it

allow for safe updates based on previous state, used for multiple updates and async functions

if your working with a stateless varaible without one property

eg. const \[car, setCar] = useState({year: 2025, make: ford, model: raptor})

updating one object will overiide all the other properties and display only one eg

setCar(year: 2026), so we make use of spread operator ... and updater func to keep prev state and update onlly one item

like this:

setCar(c => ({...c, year:2025}))

##### **the react Router**

the router allows our web app to have multiple pages and allows us to navigate from one page to another, without making requests to the server

so to use it we have to instal it

npm install react-router-dom

wrap your

with <BrowserRouter>

&nbsp; <app />

&nbsp; </BrowserRouter>

then in the app.jsx

wrap your pages with::, you have to

import { Routes, Route} from "react-router-dom";

<Routes>

&nbsp; <Routes>

&nbsp; <Route path="/" element={<Home />} />

&nbsp; <Route path="/about" element={<about />} />

&nbsp; </Routes>

</Routes>

import Link from "react-router-dom";

add navigation links in pages

<Link to="/">Home</Link>

its magical, react swaps out the componets without reaching to the server

useNavigate()

is another hook that comes with react-router-dom, it allows the system to navigate from one page to another, just like a link but the difference is a link is for users it waits for user to click the useNavigate navigates on its own.

its mostly used after login, logout or form submission.

import it

use it inside a component

like

import { useNavigate } from "react-router-dom";

&nbsp;

function Login(){

navigate = useNavigate()

const handleNavigation =()=>{

navigate("/dashbord")}

}

export ....
