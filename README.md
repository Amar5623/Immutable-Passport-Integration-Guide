# Immutable-Passport-Integration-Guide

Here is a guide that teaches you how to integrate Immutable Passport into your application:

### Table Of Contents:

1. [Introduction](https://github.com/Amar5623/Immutable-Passport-Integration-Guide/blob/main/README.md#introduction)
2. [Prerequisites](https://github.com/Amar5623/Immutable-Passport-Integration-Guide/blob/main/README.md#prerequisites)
3. [Steps](https://github.com/Amar5623/Immutable-Passport-Integration-Guide/blob/main/README.md#steps)
    1. [Creating a simple application or git cloning a repository of the simple application](https://github.com/Amar5623/Immutable-Passport-Integration-Guide/blob/main/README.md#1-creating-a-simple-application-or-git-cloning-a-repository-of-the-simple-application)
    2. [Registering the application on Immutable Developer Hub](https://github.com/Amar5623/Immutable-Passport-Integration-Guide/blob/main/README.md#2-registering-the-application-on-immutable-developer-hub)
    3. [Installing and initialising the Passport client](https://github.com/Amar5623/Immutable-Passport-Integration-Guide/blob/main/README.md#3-installing-and-initialising-the-passport-client)
    4. [Logging in a user with Passport](https://github.com/Amar5623/Immutable-Passport-Integration-Guide/blob/main/README.md#4-logging-in-a-user-with-passport)
    5. [Displaying on the app the id token, access token obtained from authenticating with Passport after login, and the user's nickname](https://github.com/Amar5623/Immutable-Passport-Integration-Guide/blob/main/README.md#5-displaying-on-the-app-the-id-token-access-token-obtained-from-authenticating-with-passport-after-login-and-the-users-nickname)
    6. [Logging out a user](https://github.com/Amar5623/Immutable-Passport-Integration-Guide/blob/main/README.md#6-logging-out-a-user)
    7. [Initiate a transaction from Passport, such as sending a placeholder string and obtaining the transaction hash](https://github.com/Amar5623/Immutable-Passport-Integration-Guide/blob/main/README.md#7-initiate-a-transaction-from-passport-such-as-sending-a-placeholder-string-and-obtaining-the-transaction-hash-to-initiate-a-transaction-from-passport-use-the-authenticate-function-provided-by-passportjs)
    8. [Creating a sample application](https://github.com/Amar5623/Immutable-Passport-Integration-Guide/blob/main/README.md#8-creating-a-sample-application-to-create-a-sample-application-follow-these-steps)
   
## Introduction
This guide will walk you through the process of integrating Immutable Passport into your application. The guide will cover the following topics:

1. Creating a simple application or git cloning a repository of the simple application
2. Registering the application on Immutable Developer Hub
3. Installing and initialising the Passport client
4. Logging in a user with Passport
5. Displaying on the app the id token, access token obtained from authenticating with Passport after login, and the user's nickname
6. Logging out a user
7. Initiate a transaction from Passport, such as sending a placeholder string and obtaining the transaction hash

## Prerequisites
Before you begin, make sure you have the following:

1. A basic understanding of JavaScript and Node.js.
2. A text editor or an IDE.
3. An account on [Immutable Developer Hub](https://hub.immutable.com/).

## Steps

### 1. Creating a simple application or git cloning a repository of the simple application

You can create a simple application or clone an existing repository of a simple application to get started. If you don't have an existing repository, you can create one by following the instructions on [GitHub Docs](https://docs.github.com/en/repositories/creating-and-managing-repositories/cloning-a-repository).

### 2. Registering the application on Immutable Developer Hub

To register your application on the Immutable Developer Hub, follow these steps:

1. Log in to your account on [Immutable Developer Hub](https://hub.immutable.com/).
2. Click on "Create Application" and fill in the required details.
3. Once you have created your application, take note of your `client_id` and `client_secret`.

### 3. Installing and initialising the Passport client

After registering your application, install and initialise the Passport client by following these steps:

1. Install `immutable-passport` using npm:
   ```
   npm install immutable-passport
   ```
2. Initialise `immutable-passport` with your `client_id` and `client_secret`:
   ```javascript
   const ImmutablePassport = require('immutable-passport');

   const immutablePassport = new ImmutablePassport({
     clientID: 'your-client-id',
     clientSecret: 'your-client-secret',
     callbackURL: 'http://localhost:3000/auth/callback',
   });
   ```

### 4. Logging in a user with Passport

To log in a user with Passport, use the `login()` function provided by Passport.js:

```javascript
app.get('/auth/immutable', passport.authenticate('immutable'));
```

You can find more information about this function in the [Passport.js documentation](https://www.passportjs.org/concepts/authentication/login/).

### 5. Displaying on the app the id token, access token obtained from authenticating with Passport after login, and the user's nickname

After logging in a user with Passport, you can display their id token, access token, and nickname on your app by accessing these values from the `req.user` object:

```javascript
app.get('/', (req, res) => {
  res.send(`Hello ${req.user.nickname}! Your id token is ${req.user.id_token} and your access token is ${req.user.access_token}.`);
});
```

You can find more information about this object in the [Passport.js documentation](https://www.passportjs.org/docs/profile/).

### 6. Logging out a user

To log out a user, use the `logout()` function provided by Passport.js:

```javascript
app.get('/logout', (req, res) => {
  req.logout();
  res.redirect('/');
});
```

You can find more information about this function in the [Passport.js documentation](https://www.passportjs.org/docs/logout/).

### 7. **Initiate a transaction from Passport, such as sending a placeholder string and obtaining the transaction hash**: To initiate a transaction from Passport, use the `authenticate()` function provided by Passport.js:

```javascript
app.get('/transaction', (req, res) => {
  immutablePassport.authenticate('transaction', (err, transactionHash) => {
    if (err) {
      console.error(err);
      res.status(500).send('Error initiating transaction');
    } else {
      console.log(`Transaction initiated with hash ${transactionHash}`);
      res.send(`Transaction initiated with hash ${transactionHash}`);
    }
  })(req, res);
});
```

You can find more information about this function in the [Passport.js documentation](https://www.passportjs.org/docs/authenticate/).

### 8. **Creating a sample application**: To create a sample application, follow these steps:

   1. Create a new directory for your application.
   2. Create a new file called `index.js` in the directory.
   3. Add the following code to `index.js`:
      ```javascript
      const express = require('express');
      const passport = require('passport');
      const session = require('express-session');
      const ImmutablePassport = require('immutable-passport');

      const app = express();

      // Initialize session middleware
      app.use(session({
        secret: 'your-secret-key',
        resave: false,
        saveUninitialized: false,
      }));

      // Initialize Passport middleware
      app.use(passport.initialize());
      app.use(passport.session());

      // Initialize Immutable Passport
      const immutablePassport = new ImmutablePassport({
        clientID: 'your-client-id',
        clientSecret: 'your-client-secret',
        callbackURL: 'http://localhost:3000/auth/callback',
      });

      // Use Immutable Passport for authentication
      app.get('/auth/immutable', passport.authenticate('immutable'));

      // Handle callback after authentication
      app.get('/auth/callback', passport.authenticate('immutable', {
        successRedirect: '/',
        failureRedirect: '/login',
      }));

      // Display user information after login
      app.get('/', (req, res) => {
        res.send(`Hello ${req.user.nickname}! Your id token is ${req.user.id_token} and your access token is ${req.user.access_token}.`);
      });

      // Log out user
      app.get('/logout', (req, res) => {
        req.logout();
        res.redirect('/');
      });

      // Initiate transaction
      app.get('/transaction', (req, res) => {
        immutablePassport.authenticate('transaction', (err, transactionHash) => {
          if (err) {
            console.error(err);
            res.status(500).send('Error initiating transaction');
          } else {
            console.log(`Transaction initiated with hash ${transactionHash}`);
            res.send(`Transaction initiated with hash ${transactionHash}`);
          }
        })(req, res);
      });

      // Start server
      app.listen(3000, () => {
        console.log('Server started on port 3000');
      });
      ```
   4. Install the required dependencies using npm:
       ```
       npm install express passport express-session immutable-passport
       ```
   5. Run the application using node:
       ```
       node index.js
       ```
