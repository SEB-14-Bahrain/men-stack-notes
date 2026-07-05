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
- initialize a node project with `npm init -y`
- install express `npm i express`

### Write Server Boilerplate

server.js
```js
const express = require('express')
const app = express()

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
app.get('/:id', function(req,res){
  res.send(`Hello user number: ${req.params.id}`)
    console.log(req.params.id)
})
```
Navigate to `http://localhost:3000` to view our server.

<img width="364" height="169" alt="image" src="https://github.com/user-attachments/assets/a1871126-1513-4743-b178-52e779b49384" />
