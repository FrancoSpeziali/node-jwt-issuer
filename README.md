# Node.js JWT Issuer

In this assignment you will be expected to write a backend system which will issue JWT tokens on login

## Getting started

This project includes some already prepared starter code. The following packages have already been specified:

> "express",
> "mongoose"

Please run `npm install` before starting

## What you will be doing

You will be creating a backend server which will create and sign JWT tokens for every user which logs in.

For this, you will have to create an endpoint to 1) register the user (create new user), 2) login the user (verify the login details of the user) and 3) logout the user (invalidate the login credentials).

During the login phase, your server will create and sign a JWT token which will be sent back to the user. Later we will store this JWT token in a `httpOnly` cookie.

## Expectations

You are expected to already have the following knowledge:
- Node and express.js
- How to run node files from the command line
- MongoDB, Mongoose
- How to create Mongoose schemas

## Tasks

## Task 1 - Create dotenv

1. Create a `.env` file which will contain your secret keys for the server

## Task 2 - prepare your server code

Setup a server in `server.js` with `express.js`

1. Use the `express.json()` middleware

2. Install & import the `cors` package and use the `cors()` middleware

> Do not use port 3000 for your server (this is used by create-react-app)

## Task 3 - Prepare your Schema

You will be creating a mongoose schema

1. Create the file `UserSchema.js`

2. Import mongoose

3. Create a mongoose schema based on the following characteristics:

    - **name** string, not required
    
    - **email** string, required
    
    - **hash** string, required

4. Export your schema

## Task 4 - Prepare your Model

1. Create the file `UserModel.js`

2. Import mongoose & the UserSchema you just created

3. Create a mongoose model based on the schema you imported

4. Export the model

## Task 5 - Connect to your MongoDB server

1. Use the `dotenv` package to load the `process.env` variables from your `.env` file

2. In the file `server.js`, connect to your server

3. Check the connection

## Task 6 - Setup the '/user' route

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

## Task 7 - Register the user

1. Create the endpoint `/register` in `user.js`. This will be a `POST` route.

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

## Task 8 - JWT Issuer

1. Create a new file called `jwtIssuer.js` inside a folder called `utils`

2. Install `jsonwebtoken`

```shell script
npm i jsonwebtoken
```

3. Import `jsonwebtoken`

4. create a function called `jwtIssuer`

This function should accept the `User` object

5. Create a variable `payload`. This should be an object with 2 keys:

`sub` - the value for this key should be the id of the user
`iat` - the value for this key should be `Date.now()` (the current time)

6. Inside this function use the following function to create a token:

    ```javascript
    jsonwebtoken.sign(payload, 'secret');
    ```
    
    Where:
    
    `payload` is an object you created in step 5
    `secret` is a string value of your choice

7. The function should return the signed token:

8. Export this function

## Task 9 - Login the user

In this assignment, we will create an endpoint to login the user, and issue a JWT token

1. Create the endpoint `/login` in `user.js`. This will be a `POST` route.

Inside your `/login` route, you can expect to receive the values, **email** and **password** from `request.body`

2. Using the `User` model, use the `findOne` method to search for the user based on the `email`

    - If the user doesn't exist, send a fail response to the user
    
    - If the user does exist use `bcrypt.compare()`to compare the password with the hash in the database
    
    - If the password does not match, send a fail response to the user

3. Import the `jwtIssuer` function from the `utils` folder

4. If the user exists, and the password matched, use the `jwtIssuer` function to create a jwt token, and send this as a response to the user

5. Test your code

## Task 10 - Setting the httpOnly cookie

It's very important to keep the JWT token as secure as possible. The token is a portable object of data which authorises a particular user to the server. If a hacker were able to get hold of this, they could use it to access information they shouldn't.

A `httpOnly` cookie is controlled by the backend, but is handled by the browser. This means the frontend JavaScript can not access it, making it harder for hackers to get hold of.

There are other ways to secure a JWT token, but we will focus on this approach for now.

1. Modify your `/login` route, so that your response also uses the `express.js` `cookie()` method.

The `cookie()` method takes 3 parameters, `name`, `value` and `options`.

- For `name` use the string `jwt`
- For `value` pass in your JWT token
- For `options`, use the object:
```javascript
{
    httpOnly: true,
    sameSite: 'lax'
}
```

This will instruct the client (browser) to set the JWT token as a `httpOnly` cookie

Once the user logs in with this route, all future requests (with the `credentials: 'include'` option) will include this `httpOnly` cookie.

Example:

```javascript
 response
   .cookie("jwt", token, {
     httpOnly: true,
     sameSite: "lax",
   })
```

Where `token` is your JWT token.

Research: [Express.js response.cookie() method [en]](http://expressjs.com/en/4x/api.html#res.cookie)

## Task 11 - Logout

We can logout the user by invalidating the JWT token. There are 2 ways we can do this:

   1) Setting an expiry date on the token when we generate it
   2) Create an endpoint which will run the `express.js` `clearCookie()` method

We will focus on point 2)

1. Create the endpoint `/logout` in `user.js`. This will be a `GET` route.

2. In your response, use the `clearCookie()` method to remove the `httpOnly` cookie we set when the user logs in. You must use the same options that we use when setting the cookie.

Example:

```javascript
 response
   .clearCookie("jwt", {
     httpOnly: true,
     sameSite: "lax",
   })
```

Research: [Express.js response.clearCookie() method [en]](https://expressjs.com/en/4x/api.html#res.clearCookie)

## Task 12 - Create a React frontend

In this assignment you will create a React frontend which will have a `register` and `login` form

1. Create a Register form with the following fields:

`name` - not required
`email`
`password`

2. Create a login form with the following fields:

`email`
`password`

For the login, it is important that we use the `credentials: 'include'` option when using our `fetch` request (`withCredentials: true` if you are using Axios), otherwise the `httpOnly` cookie can not be set.
