javaScript

##### variables

unlike java, javascript automatically knows the datatypes of the variable you just have to declare the variable

there are 3 ways

var name = "tino"

let = age = 24

const PI = 3.14127 // its good practise to make the letters uppercase if ony it is a preimitive datatype, numbers and booleans

##### arithmetica operators

+,-,\*,/,%

==,

##### taking user input

there are 2 ways to take user input, WINDOW prompt, html text box

let username;
username = window.prompt("whats your username")
console.log(username);

<label>username</label>
<input id="myname" />
<button id="mybutton">click me</button>

let usename;
document.getElementById("mybutton).onClick = function(){
username = document.getElementById("myname").value
console.log(username)
}

#### type conversion

let name = "tino"
let age = 24

name = number(name)
age = string(age)
name = boolean(name)

when taking userinput it always comes as a string so if you want to work with numbers you have to convert

#### math

is a bult in method that allows to perform maths operations
math.round()
math.floor()
math.ceil()
math.trunc()
math.pow()
math.sqrt
math.log
math.sin,cos,tan()
math.abs
math.min
math.max

#### if statements

same old

if(condition){

}else if(conditon 2){

}else{

}

here order matter some conditons might be both true and leading to different outcomes the program goes line by line
just plce them with desired precidence

#### ternary operator

a small if else for small staff, its cool
let age = 21
let result = age > 18 ? "your miner" : "your adult"

#### switches

let day = 2

switch(day){
case 1:
console.log("monday")
break;
case 2:
console.log("tuesday")
break;
..
default :
console.log("ibvapa")

}
break is to make the program stop when the case is found

#### string methods

## ES6+ FEATURES

1. arrow functions
2. template literals
3.
