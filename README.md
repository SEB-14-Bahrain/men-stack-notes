# MEN STACK NOTES

| CRUD Action | RESTful Action | HTTP Method | Definition |
| ----------- | -------------- | ----------- | ------------------------ |
|  | `new` | `GET` | Show a form to create a new item |
| Create | `create` | `POST` | Add a new item to the database |
| Read | `index` | `GET` | Show all items |
| Read | `show` | `GET` | Show one specific item |
| | `edit` | `GET` | Show a form to edit an existing item |
| Update | `update` | `PUT` | Save changes to an existing item |
| Delete | `delete` | `DELETE` | Remove an item from the database |


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
