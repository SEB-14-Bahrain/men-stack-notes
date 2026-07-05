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


