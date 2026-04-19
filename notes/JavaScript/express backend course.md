express backend course

\*this course uses module not common js\*

its a node library that's used to create a node.js server

basically a server is a computer/code running and listening for requests from a specified PORT

to use express obv you need node

npm init -y

this is to initialise a node project

npm install express

create your entry point usually server.js or index.js

```js
import express from 'express' //import express so you can use it

const app = express()  //initialise the app to use express

PORT = 2015 // can be any 4 digit number



app.listen(PORT, ()=>{

&nbsp;console.log('server running on {PORT}')

})
```

//app.listen is placed at the bottom of this file, its there to constantly listen to all requests made to the PORT

so the frontend communicate with the server via predefined methods which are GET, POST, DELETE,

so the server should anticipate all the moves and be ready to give a corresponding response, well you cant just listen for no reason

the route or end point is a specific part of the url that specifies what the frontend is requesting or responding

a route is known with /

/ is the home or the first end point on the url

/login the login endpoint it means the user whats to login so server should server appropriate data

/dashboard you get the drill.

so as ive said we need to anticipate and take care of all the client requests

app.get('/route', (req, res)={

whatever the fuck server should server

console.log(''hey)

res.send('hey you')

res.sedstatus(200)

})

so we can have different methods, app.post, app.delete

ok the backend usually serve 2 different things

1 websites

2 it acts as an api

whats that, when the backend is responding with a website it happens when someone enters a url and expects a website to be served

an api is when the backend is mostly sending data, that you get to interact with on an already served interface for example when you want to register

you make the post req and the backend knows what to do and it give you a corresponding response, or when you want to view results, you send a get

the api fetches tha data and it gives you.

#### serving static pages in express

you have to get the url of the running file, convert it to a normal path

store in \_\_dirname(a predefined js variable to store currect directory)

thenjoin dirname with the dir of where you pages are usually public

like this

&nbsp;const \_\_filename = fileURLToPath(import.met.url) //this gets the file url

and file to path converts it to a path

then dirname(\_\_filename ) //chops off the file where it took from

then path.join(\_\_dirname, '..public') //to get full directory name where staff if

import path, { dirname } from 'path'

import { fileURLToPath } from 'url'

const \_\_filename = fileURLToPath(import.meta.url)

const \_\_dirname = dirname(\_\_filename)

app.use(express.static(path.join(\_\_dirname, '.. public')))

app.get('/', (req, res)=>{

&nbsp;res.sendFile(path.join(\_\_dirname, '..public','index.html')

})

app.listen(PORT,()=>{

console.log(''server started running on ${PORT})

})
