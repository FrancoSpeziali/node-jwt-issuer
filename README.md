# Node.js JWT Issuer

In this assignment you will be expected to write a backend system which will issue JWT tokens on login

## Getting started

This project includes some already prepared starter code. The following packages have already been specified:

> "express": "^4.17.1",
> "mongoose": "^5.10.11"

Please run `npm install` before starting

## Assignments

## Assignment 1 - Create dotenv

1. Create a `.env` file which will contain your secret keys for the server

## Assignment 2 - prepare your server code

Setup a server in `server.js` with `express.js`

1. Use the `express.json()` middleware

2. Install & import the `cors` package and use the `cors()` middleware

> Do not use port 3000 for your server

## Assignment 3 - Prepare your Schema

You will be creating a mongoose schema

1. Create the file `UserSchema.js`

2. Import mongoose

3. Create a mongoose schema based on the following characteristics:

    - **name** string, not required
    
    - **email** string, required
    
    - **hash** string, required

4. Export your schema

## Assignment 4 - Prepare your Model

1. Create the file `UserModel.js`

2. Import mongoose & the UerSchema you just created

3. Create a mongoose model based on the schema you imported

4. Export the model

## Assignment 5 - Connect to your MongoDB server

1. Use the `dotenv` package to load the `process.env` variables from your `.env` file

2. In the file `server.js`, connect to your server

3. Check the connection

## Assignment 6 - Setup the '/user' route

1. Create a file `user.js` in a folder `routes`

This will contain all the user routes

2. Import `express`

3. Use the following code to create a route

```javascript
const router = express.Router();
```

4. Export `router` from `user.js`

5. Inside `server.js`, import `router` from `user.js`

6. Use `app.use()` to redirect all `/user` routes to the `router` variable you exported from `user.js`

## Assignment 7 - Register the user

1. Create a route `/register` in `user.js`. This will be a `POST` route.

Inside your `/register` route, you can expect to receive the values **name**, **email** and **password** from `request.body`

2. Import your `User` model into `user.js`

3. We have to check if the user already exists, before registering them

Using the `User` model, use the `findOne` method to search for the user based on the `email` address

    - If the user exists, send a fail response to the user
    - If the user doesn't exist, we will create the user
    
> Note: We are using the `email` for the username

4. Install bcrypt

```shell script
npm i bcrypt
```

5. Import `bcrypt`

6. Use bcrypt to create a hash of the password

7. If the user doesn't exist then create the user. Using the `User` model, save the user registration data to the database:

`name`
`email`
`hash` - this should be the hashed version of the password you created in step 6

8. Test your code

## Assignment 8 - JWT Issuer

1. Create a new file called `jwtIssuer.js` inside a folder called `utils`

2. Install `jsonwebtoken`

```shell script
npm i jsonwebtoken
```

3. Import `jsonwebtoken`

4. create a function called `jwtIssuer`

This function should accept the `User` object

5. Create the following variable:

```javascript
const expiresIn = '1d';
```

5. Create a variable `payload`. This should be an object with 2 keys:

`sub` - the value for this key should be the id of the user
`iat` - the value for this key should be `Date.now()` (the current time)

6. Inside this function use the following function to create a token:

    ```javascript
    jsonwebtoken.sign(payload, 'secret')`;
    ```
    
    Where:
    
    `payload` is an object you created in step 5
    `secret` is a string value of your choice

7. The function should return an object:

```javascript
token: 'Bearer ' + signedToken,
expiresIn
```

Where:

`signedToken` is the token you created in step 6
`expiresIn` is the variable you created in step 5

8. Export this function

## Assignment 9 - Login the user

In this assignment, we will create a route to login the user, and issue a JWT token

1. Create a route `/login` in `user.js`. This will be a `POST` route.

Inside your `/login` route, you can expect to receive the values, **email** and **password** from `request.body`

2. Using the `User` model, use the `findOne` method to search for the user based on the `email`

    - If the user doesn't exist, send a fail response to the user
    
    - If the user does exist use `bcrypt.compare()`to compare the password with the hash in the database
    
    - If the password does not match, send a fail response to the user

3. Import the `jwtIssuer` function from the `utils` folder

4. If the user exists, and the password matched, use the `jwtIssuer` function to create a jwt token, and send this as a response to the user

5. Test your code

## Assignment 10 - Create a React frontend

In this assignment you will create a React frontend which will have a `register` and `login` form

1. Create a Register form with the following fields:

`name` - not required
`email`
`password`

2. Create a login form with the following fields:

`email`
`password`
