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
- create server file `touch server.js`
- create a `.gitignore` file
- initialize a node project with `npm init -y`
- install express and morgan `npm i express morgan`

### Add `node_modules` to `.gitignore`

.gitignore
```bash
node_modules
.env
```

### Write Server Boilerplate

server.js
```js
const express = require('express')
const morgan = require('morgan')

const app = express()

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

- install ejs with `npm i ejs`
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

Configure our server to look inside of the `public` folder for static files:
```js
// require the path from node at the top
const path = require('path')

// use static middleware with other middlware like morgan
app.use(express.static(path.join(__dirname, "public")))
```

- Link the stylesheet in the head of our `html` files (inside `nav` partial if we're using partials)
```ejs
<link rel="stylesheet" href="/stylesheets/style.css">
```

#### Images

- Create an `images` folder inside of our `public` folder
- Link to images like normal: `<img src="/images/family.jpg" alt="A happy family">`

## Adding MongoDB and Mongoose

Our application can currently display pages, but it cannot permanently store information.

We will use:

* **MongoDB Atlas** to host our database
* **Mongoose** to communicate with MongoDB
* **dotenv** to protect our database connection string

### Install Mongoose and dotenv

Stop the server with `Ctrl + C`, then install the packages:

```bash
npm i mongoose dotenv
```

## Protecting Environment Variables

Create a `.env` file in the root of the project:

```bash
touch .env
```

The `.env` file will hold information that should not be uploaded to GitHub.

Add `.env` and `node_modules` to `.gitignore`:

```plaintext
node_modules
.env
```

> 🚨 Never commit your `.env` file to GitHub. It contains private database credentials.

### Add the MongoDB connection string

Add the MongoDB Atlas connection string to `.env`:

```plaintext
MONGODB_URI=mongodb+srv://<username>:<password>@<cluster-url>/fruits?retryWrites=true&w=majority
```

There should be no spaces around the `=` sign.

The `/fruits?` part names the database for this application.

## Connecting the Server to MongoDB

Load `dotenv` at the top of `server.js`:

```js
const dotenv = require('dotenv').config() // making .env file available
```

This reads the variables from `.env` and adds them to `process.env`.

Import Mongoose:

```js
const mongoose = require('mongoose')
```

Connect Mongoose to MongoDB and add a connection event listener so we know when the connection works:

```js
// connect us to MongoDB using connection string in .env
mongoose.connect(process.env.MONGODB_URI)
mongoose.connection.on('connected', () => {
    console.log(`Connected to MongoDB ${mongoose.connection.name} 🥭`)
})
```

Run the server:

```bash
nodemon server.js
```

We should see messages similar to:

```plaintext
Connected to MongoDB fruits
Listening on port 3000 💛
```

## Creating a Mongoose Model

A **model** represents one type of data in our application.

Our application will have a `Fruit` model.

Create a `models` directory and a `fruit.js` file:

```bash
mkdir models
touch models/fruit.js
```

Model filenames should normally be singular not plural.

### Create the Fruit schema

The completed model should look like this:

```js
// models/fruit.js

const mongoose = require('mongoose')

const fruitSchema = new mongoose.Schema({
    name: String,
    isReadyToEat: Boolean,
})

const Fruit = mongoose.model('Fruit', fruitSchema)

module.exports = Fruit
```

## Importing the Model

Import the `Fruit` model into `server.js`:

```js
const Fruit = require('./models/fruit.js')
```

The model gives our controllers access to Mongoose methods such as:

```js
Fruit.create()
Fruit.find()
Fruit.findById()
Fruit.findByIdAndUpdate()
Fruit.findByIdAndDelete()
```

At this point, we are only using `Fruit.create()`.

## Creating the New Fruit Page

Creating a fruit requires two routes:

| RESTful Action | HTTP Method | Route         | Responsibility                      |
| -------------- | ----------- | ------------- | ----------------------------------- |
| `new`          | `GET`       | `/fruits/new` | Display the form                    |
| `create`       | `POST`      | `/fruits`     | Process the form and save the fruit |

The `new` route does not save anything to the database. It only displays the form.

### Create the view

Create a `new.ejs` file inside `views`:

```bash
touch views/new.ejs
```

### Create the new route

Add the route to `server.js`:

```js
// GET /fruits/new (displays form for creating fruit)
app.get('/fruits/new', async (req, res) => {
    res.render('new.ejs')
})
```

Navigate to:

```plaintext
http://localhost:3000/fruits/new
```

---

## Building the New Fruit Form

Add the form to `views/new.ejs`:

```ejs
<%- include('./partials/nav') %>

    <h1>Create a New Fruit</h1>

    <form action="/fruits" method="POST">
        Name:
        <input type="text" name="name" />

        <br />
        <div class="checkbox-div">
            Ready to Eat?
            <input type="checkbox" name="isReadyToEat"/>
        </div>

        <button type="submit">Add Fruit</button>
    </form>
</body>
</html>
```

The form has two important attributes:

```ejs
<form action="/fruits" method="POST">
```

* `action="/fruits"` tells the browser where to send the data.
* `method="POST"` tells the browser that we are creating new data.

**The `name` attributes must match the keys in our Mongoose schema:**

```ejs
name="name"
name="isReadyToEat"
```

These match:

```js
const fruitSchema = new mongoose.Schema({
    name: String,
    isReadyToEat: Boolean,
})
```

> 💡 The input `name` is used as the property name inside `req.body`.


## Reading Form Data with Middleware

Express does not automatically read data sent by an HTML form.

Add this middleware before the routes in `server.js`:

```js
app.use(express.urlencoded({ extended: false }))
```

This middleware:

1. Reads the submitted form data.
2. Converts the data into a JavaScript object.
3. Places the object on `req.body`.

For example, submitting the fruit form may create:

```js
{
    name: 'Mango',
    isReadyToEat: 'on'
}
```

Without `express.urlencoded()`, `req.body` will be `undefined`.


## Creating the Create Route

Add the `POST /fruits` route below the `new` route:

```js
// POST /fruits
app.post('/fruits', async function (req, res) {
    console.log(req.body)

    res.redirect('/')
})
```

Submit the form and check the terminal.

We should see the submitted data:

```js
{
    name: 'Mango',
    isReadyToEat: 'on'
}
```

At this point, the route receives the form data, but it does not save it yet.

## Preparing Checkbox Data

HTML checkboxes do not automatically send boolean values.

When checked, the checkbox sends:

```js
'on'
```

When it is not checked, the property is missing from `req.body`.

Our Mongoose schema expects a boolean

```js
isReadyToEat: Boolean
```

So we must convert the checkbox value with an `if` statement before moving on:

```js
// Convert the checkbox value before saving
    if (req.body.isReadyToEat === 'on') {
        fruitData.isReadyToEat = true
    } else {
        fruitData.isReadyToEat = false
    }
```

## Saving a Fruit to MongoDB

Use `Fruit.create()` to save the submitted data:

```js
// POST /fruits (creates fruit in database)
app.post('/fruits', async (req, res) => {
    const fruitData = {}
    fruitData.name = req.body.name

// Convert the checkbox value before saving
    if (req.body.isReadyToEat === 'on') {
        fruitData.isReadyToEat = true
    } else {
        fruitData.isReadyToEat = false
    }

    let createdFruit = await Fruit.create(fruitData)

    res.redirect('/')
})
```

`Fruit.create()` is asynchronous because the application must communicate with an external database.

We use `await` so the application waits for MongoDB to finish creating the document before redirecting the user.


## Verify the Fruit in MongoDB Atlas

After submitting the form:

1. Open MongoDB Atlas.
2. Open the database deployment.
3. Select **Browse Collections**.
4. Open the `fruits` database.
5. Open the `fruits` collection.

We should see a document similar to:

```js
{
    _id: ObjectId('6a51e2a614b8a7dd887df8b3'),
    name: 'Mango',
    isReadyToEat: true,
    __v: 0
}
```

MongoDB automatically adds `_id`.

Mongoose automatically adds `__v`.




