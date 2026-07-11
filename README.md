# MEN STACK NOTES

## Example Fruits CRUD

|HTTP<br>Method|URL<br>Endpoint|Purpose|
|---|---|---|
| GET | /fruits/new | View a form for submitting a fruit (be sure to define this route before the show route)|
| POST | /fruits | Handle the new fruit form being submitted |
| GET | /fruits | View all the fruits|
| GET | /fruits/:fruitId |  View the details of any fruit |
| DELETE | /fruits/:fruitId| Delete a fruit (restrict to user who submitted the fruit) |
| GET | /fruits/:fruitId/edit | View a form for editing a fruit (restrict to user who submitted the fruit) |
| PUT | /fruits/:fruitId|  Handle the edit fruit form being submitted (restrict to user who submitted the fruit) |

## The Create Request Cycle

When the user creates a fruit, the request follows these steps:

```plaintext
GET /fruits/new
        ↓
Express renders new.ejs
        ↓
The user completes the form
        ↓
The form sends POST /fruits
        ↓
express.urlencoded() creates req.body
        ↓
The controller converts the checkbox value
        ↓
Fruit.create(req.body) saves the document
        ↓
Express redirects to /fruits/new
```
## Current Project Structure

At this point, the application should will a structure like this:

```plaintext
men-stack-crud-app-fruits/
├── models/
│   └── fruit.js
├── public/
│   ├── images/
│   └── stylesheets/
│       └── style.css
├── views/
│   ├── partials/
│   │   └── nav.ejs
│   ├── new.ejs
│   └── home.ejs
├── .env
├── .gitignore
├── package-lock.json
├── package.json
└── server.js
```


## SETUP

- create a directory
- create our first files `touch server.js .gitignore .env`
- initialize a node project with `npm init -y`
- install applications `npm i express morgan ejs mongoose dotenv`
- open our project with `code .`

### Add `node_modules` to `.gitignore`

.gitignore
```bash
node_modules
.env
```

### Add the MongoDB connection string to `.env`:

```plaintext
MONGODB_URI=mongodb+srv://<username>:<password>@<cluster-url>/fruits?retryWrites=true&w=majority
```
_Remember to change your database name in the connection string_


### Write Server Boilerplate

server.js
```js
const express = require('express')
const morgan = require('morgan')
const path = require('path')

const app = express()

app.use(express.static(path.join(__dirname, "public"))) // use static middleware with other middlware like morgan
app.use(express.urlencoded({ extended: false })) // need this for reading form data
app.use(morgan('dev'))

app.listen(3000, function(){
    console.log('Listening on port 3000 💛')
})
```

Run the server with `nodemon server.js`
<img width="751" height="181" alt="Screenshot 2026-07-05 at 11 22 36 AM" src="https://github.com/user-attachments/assets/5d613f26-670f-47ce-9d54-455cdbe3b6b7" />

Navigate to `http://localhost:3000` to view our server.

Use `ctrl + c` to stop the server in the terminal.

### Creating a Test Route

```js
app.get('/test', function(req, res) {
    res.send('this is a test ✨')
})
```
<img width="330" height="145" alt="Screenshot 2026-07-05 at 12 30 29 PM" src="https://github.com/user-attachments/assets/5afaaa31-7117-4e82-99cf-79316aed6b9a" />

Navigate to `http://localhost:3000/test`

### Using request parameters

```js
app.get('/:userId', function(req, res){
    res.send(`Hello user number:${req.params.userId}`)
    console.log(req.params.userId)
})
```
Navigate to `http://localhost:3000/2490`

<img width="532" height="179" alt="Screenshot 2026-07-05 at 2 12 17 PM" src="https://github.com/user-attachments/assets/4a428ac3-5fdd-4917-ab90-38e9cd39fad3" />

## Rendering EJS

- create a `views` directory
- create an `.ejs` file like `home.ejs`
- add html boilerplate with `!`

home.ejs
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Home</title>
</head>
<body>
    <h1>This is an ejs page! 🚀</h1>
</body>
</html>
```

- render `ejs` page using a controller like this one:

```js
app.get('/', function(req, res){
    res.render('home.ejs')
})
```
<img width="632" height="311" alt="Screenshot 2026-07-06 at 12 39 56 PM" src="https://github.com/user-attachments/assets/be240674-1cf5-4b60-abe7-2c3799f72296" />

### EJS Syntax

To use javaScript in an ejs file, I need a scriplet tag:
```ejs
<% let user = 'nabila' %>
```

To display javaScript values from an ejs file, I need an output tag:
```ejs
<%= user %>
```

### Pass data from the controller

Use the locals object inside the render method:
```js
res.render('home.ejs', {
    title: 'Home Page',
})
```
Now I can use the `title` variable in my `home.ejs` file.

home.ejs
```ejs
<h1><%= title %></h1>
```

### Using `forEach` in `ejs`

```ejs
<ul>
    <% inventory.forEach(function(item){ %>
    <li><%= item.name %></li>
    <% }) %>
</ul>
```
<img width="597" height="313" alt="Screenshot 2026-07-06 at 2 59 14 PM" src="https://github.com/user-attachments/assets/9b8c861c-680d-456b-98fd-b5a0522a9af1" />

### Creating dynamic links to a `show` page

`item.name` is dynamically showing up.  The link is also dynamically changing with the item. (see `forEach` above)
```ejs
<a href="/<%= item.id %>"> <%= item.name %> </a>
```
We should see the URL change in the browser.

### Reusable Nav bar with partials
- Create a folder called `partials` inside of `views`
- Create a file called `nav.ejs` inside of `partials`

Should include the opening `html`, `head`, and `body` tags:
```ejs
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title><%= title %></title>
    <link rel="stylesheet" href="/stylesheets/style.css">
</head>
<body>
    <nav>
        <a href="/">Home</a>
    </nav>
```

Include the `nav` in other files with this statement:
```ejs
<%- include('./partials/nav') %>
```

### Adding CSS & images with `public`

#### CSS

- Create a folder called `public` 
- Create a folder called `stylesheets` inside of `public`
- Create a file called `style.css` inside of `stylesheets`

- Link the stylesheet in the head of our `html` files (inside `nav` partial if we're using partials)
```ejs
<link rel="stylesheet" href="/stylesheets/style.css">
```

#### Images

- Create an `images` folder inside of our `public` folder
- Link to images like normal: `<img src="/images/family.jpg" alt="A happy family">`

## 🥭 Adding MongoDB and Mongoose

### Connect to MongoDB

At the top of `server.js`, load `dotenv` and Mongoose:

```js
const dotenv = require('dotenv').config()
const mongoose = require('mongoose')
```

Connect to MongoDB _after_ `const app = express()`:

```js
const app = express()

// connect to mongoDB
mongoose.connect(process.env.MONGODB_URI)

mongoose.connection.on('connected', () => {
    console.log(`Connected to MongoDB ${mongoose.connection.name} 🥭`)
})
```
<img width="553" height="96" alt="Screenshot 2026-07-11 at 10 12 22 AM" src="https://github.com/user-attachments/assets/92af3e8a-4d56-460a-ade8-7e76226cf139" />


## Creating a Fruit in the Database

### Create a Fruit model

Create a `models` folder and a `fruit.js` file:

```bash
mkdir models
touch models/fruit.js
```

Add the Fruit schema and model:

```js
const mongoose = require('mongoose')

const fruitSchema = new mongoose.Schema({
    name: String,
    isReadyToEat: Boolean,
})

const Fruit = mongoose.model('Fruit', fruitSchema)

module.exports = Fruit
```

Import the model into `server.js` _after_ your MongoDB Connection string:

```js
    console.log(`Connected to MongoDB ${mongoose.connection.name} 🥭`)
})

// import Fruit model here
const Fruit = require('./models/fruit.js')
```

## Create the New Fruit Form

Create `new.ejs` inside the `views` folder:

```plaintext
views/
├── home.ejs
└── new.ejs
```

Add the form:

```ejs
<h1>Add a Fruit</h1>

<form action="/fruits" method="POST">
    Fruit name:
    <input type="text" name="name">

    Ready to eat?
    <input type="checkbox" name="isReadyToEat">

    <button type="submit">Add Fruit</button>
</form>
```

The input `name` values become keys inside `req.body`.

For example:

```js
req.body.name
req.body.isReadyToEat
```

### Display the New Fruit Form

Create the `new` route:

```js
// GET /fruits/new
app.get('/fruits/new', async (req, res) => {
    res.render('new.ejs')
})
```

Visit:

```plaintext
http://localhost:3000/fruits/new
```

The same form without and with CSS:

<img width="300" height="260" alt="Screenshot 2026-07-11 at 10 42 44 AM" src="https://github.com/user-attachments/assets/287c4471-26f0-4f05-b7ef-a43b677bdf78" />
<img width="304" height="260" alt="Screenshot 2026-07-11 at 10 39 33 AM" src="https://github.com/user-attachments/assets/fdd6558c-b038-4b47-a4d1-1d6e20969f96" />


## Create a Fruit from the Form

Create the `POST /fruits` route:

```js
// POST /fruits
app.post('/fruits', async (req, res) => {
    const fruitData = {}

    fruitData.name = req.body.name

    // Converts the 'on' into true for our isReadToEat boolean on our model
    if (req.body.isReadyToEat === 'on') {
        fruitData.isReadyToEat = true
    } else {
        fruitData.isReadyToEat = false
    }

    const createdFruit = await Fruit.create(fruitData)

    res.redirect('/')
})
```

The route:

1. Receives the form data through `req.body`.
2. Creates a new `fruitData` object.
3. Converts the checkbox value into a boolean.
4. Uses `Fruit.create()` to save the fruit.
5. Redirects the user to the home page.

<details><summary><strong>Why We Convert the Checkbox</strong></summary>



A checked HTML checkbox sends the string:

```js
'on'
```

An unchecked checkbox does not send a value.

Our schema (model) expects a boolean:

```js
isReadyToEat: Boolean
```

We convert the checkbox value before adding it to the database:

```js
if (req.body.isReadyToEat === 'on') {
    fruitData.isReadyToEat = true
} else {
    fruitData.isReadyToEat = false
}
```

</details>

### Check the Database

1. Open MongoDB Atlas.
2. Select **Browse Collections**.
3. Open the `fruits_db` database.
4. Open the `fruits` collection.

We should see a document similar to:
<img width="1163" height="154" alt="Screenshot 2026-07-11 at 10 35 26 AM" src="https://github.com/user-attachments/assets/429b2d57-242c-4d03-8d42-322796449f51" />

MongoDB automatically adds `_id` and `__v`.





