---
title: Hello, Blockstack Tutorial
description: Tutorial on building a hello world application.
image: /images/visuals/hello-blockstack-tutorial.png
youtube: https://www.youtube.com/embed/UbZ6PlX5rF8
---

In this tutorial, you build a simple application on Blockstack. The application
is a single-page application (SPA) that runs completely client-side. The
application has no backend API to talk to, other than the identity and storage
API that the user provides. In this sense, the application is a completely
decentralized, server-less application.

For this tutorial, we will use the following tools:

- `npm` to manage dependencies and scripts
- `browserify` to compile node code into browser-ready code
- `blockstack.js` to authenticate the user and work with the user's identity/profile information


## About this tutorial and the prerequisites you need

At minimum, Blockstack requires macOS High Sierra. This tutorial was written for
a user running macOS High Sierra 10.13.4. The application you build is a
React.js application that is completely decentralized and server-less.  While
not strictly required to follow along, basic familiarity with React.js is
helpful.

When complete, the app is capable of the following:

- authenticating users using Blockstack
- posting new statuses
- displaying statuses in the user profile
- looking up the profiles and statuses of other users

The basic identity and storage services are provided by `blockstack.js`. To test
the application, you need to have already registered a Blockstack ID.

The tutorial relies on the `npm` dependency manager. Before you begin, verify
you have installed `npm` using the `which` command to verify.

```bash
$ which npm
/usr/local/bin/npm
```

If you don't find `npm` in your system, [install
it](https://www.npmjs.com/get-npm). Finally, if you get stuck at any point
while working on the tutorial, the completed [source code is available for
you]() to check your work against.

Finally, make sure you have [created at least one Blockstack ID](ids-introduction.md#create-an-initial-blockstack-id). You'll use this ID to interat with the application.

## Use npm to install Yeoman and the Blockstack App Generator

You use `npm` to install Yeoman. Yeoman is a generic scaffolding system that
helps users rapidly start new projects and streamline the maintenance of
existing projects.


1. Install Yeoman.

    ```bash
    npm install -g yo
    ```
2. Install the Blockstack application generator.

    ```bash
    npm install -g generator-blockstack
    ```

<!-- Need to find out if user is required to have React installed before running Yeoman. Doesn't appear to be the case. -->

## Generate an initial Blockstack application

In this section, you build an initial React.js application called `hello-world-tutorial`.

1. Create the `hello-world-tutorial` directory.

    ```bash
    mkdir hello-world-tutorial
    ```

2. Change into your new directory.

    ```bash
    cd hello-world-tutorial
    ```

3. Use Yeoman and the Blockstack application generator to create your initial `hello-world-tutorial` application.

    ```bash
    yo blockstack
    ```

    You should see several interactive prompts.

    ```bash
    $ yo blockstack

         _-----_     ╭──────────────────────────╮
        |       |    │      Welcome to the      │
        |--(o)--|    │      Blockstack app      │
       `---------´   │        generator!        │
        ( _´U`_ )    ╰──────────────────────────╯
        /___A___\   /
         |  ~  |
       __'.___.'__
     ´   `  |° ´ Y `

    ? Are you ready to build a Blockstack app in React? (Y/n)
    ```

4. Respond to the prompts to populate the initial app.

    After the process completes successfully, you see a prompt similar to the following:

    ```bash
    ...
   create public/icon-192x192.png
   create public/index.html
   create public/robots.txt
   create public/manifest.json


    I'm all done. Running npm install for you to install the required dependencies. If this fails, try running the command yourself.
    ```

    Depending on your environment you may have some problems with the npm packages. Go ahead and fix these before continuing.

## Understand the basic application structure

The initial application you create is a generic Javascript application you run
with a local express node. Before you continue, take a moment to examine the
structure of this generic application structure:

| File             | Description                       |
|------------------|-----------------------------------|
| .editorconfig    | Sets universal values for editor. |
| .gitignore       | Git configuration file.           |
| firebase.json    | Configuragion for mobile application. |                         |
| package.json     | Specifies required packages.                          |
| requires.js      | A Javascript module loader.                          |
| server.js        | Simple static server configuration.                          |

In the `public` folder you find these files:

| File             | Description                       |
|------------------|-----------------------------------|
| app.css   | Contains application styles.                         |
| app.js    | Main application file.                          |
| boostrap.min.css | Minifield css for production.                          |
| icon-192x192.png | Application icon                          |
| index.html       | Single page.                          |
| manifest.json    | Tells the browser about the application and how it should behave when 'installed' on the users mobile device or desktop.                          |
| robots.txt       | Configures crawling and indexing.                          |

The simple static file server in the `server.js`file serves all of the files in
the `/public` directory, including `index.html`, `app.js`, `bootstrap.min.css`
and `app.css`. The main file of the application is in the `app.js`. It contains
the majority of the application logic.

## Start the server and view the application

When you start the server, it will create a Node.js server, start it locally,
and open your browser 'http://localhost:5000'.  From the root of your new application directory:

1. Start the application server.

    ```bash
    npm start
    ```

    The first time you run it, your system prompts you to accept incoming connections.

    ![Network Connection](./images/network-connections.gif)

2. Choose **Allow**.

7. Open your browser to `http://localhost:8080`.

   You should see a simple React app.

   ![](images/initial-app.gif)

8. Choose **Sign In with Blockstack**.

   The application detects whether the user has the client edition installed or
   not. This is done automatically by the Blockstack API, more about this later.
   The next step depends on the users' current state.

   | Only using web app             | Has client edition installed                       |
   |------------------|-----------------------------------|
   |   ![](images/login-choice.gif) | ![](images/login.gif) |


    The application tells the user it will **Read your basic info**.

    ![](images/login.gif)

Leave your new application running and move onto the next section.



### Part 2: Serving the App




### Part 3: Main App Code



As you can see, all of the code in the file is wrapped in an event listener that
waits until the DOM content has been loaded:

```js
document.addEventListener("DOMContentLoaded", function(event) {
})
```

Inside of this, we have a sign in button handler that creates an auth request
and redirects the user to sign in:

```js
document.getElementById('signin-button').addEventListener('click', function() {
  blockstack.redirectUserToSignIn()
})
```

We also have a sign out button handler that deletes the local user data and signs the user out:

```js
document.getElementById('signout-button').addEventListener('click', function() {
  blockstack.signUserOut(window.location.origin)
})
```

Next, we have a function for showing the user's profile:

```js
function showProfile(profile) {
  var person = new blockstack.Person(profile)
  document.getElementById('heading-name').innerHTML = person.name()
  document.getElementById('avatar-image').setAttribute('src', person.avatarUrl())
  document.getElementById('section-1').style.display = 'none'
  document.getElementById('section-2').style.display = 'block'
}
```

Then, we have logic for signing the user in and displaying the application.

Note that there are 3 main states the user can be in:

- The user is already signed in
- The user has a sign in request that is pending
- The user is signed out

We express that as follows:

```js
if (blockstack.isUserSignedIn()) {
  // Show the user's profile
} else if (blockstack.isSignInPending()) {
  // Sign the user in
} else {
  // Do nothing
}
```

With the first condition (when the user is signed in), we load the user data from local storage and then display the profile. With the second condition (when the user has a pending sign in request), we sign the user in and redirect the user back to the home page.

```js
if (blockstack.isUserSignedIn()) {
  var profile = blockstack.loadUserData().profile
  showProfile(profile)
} else if (blockstack.isSignInPending()) {
  blockstack.handlePendingSignIn().then(function(userData) {
    window.location = window.location.origin
  })
}
```

### Part 4: App Manifest

The app manifest file (`/public/manifest.json`) contains configurations for your app that dictate how it will be displayed in auth views and on user home screens.

The `manifest.json` file should look like this:

```json
{
  "name": "Hello, Blockstack",
  "start_url": "localhost:5000",
  "description": "A simple demo of Blockstack Auth",
  "icons": [{
    "src": "https://helloblockstack.com/icon-192x192.png",
    "sizes": "192x192",
    "type": "image/png"
  }]
}
```

Keep it as is or fill it in with new information that describes your app.

### Part 5: Source Control

To complete the tutorial, save your app code by pushing it to GitHub.

First, create a simple git repo:

```bash
git init
```

Next, add and commit all of the files:

```bash
git add . && git commit -m "first commit"
```

Then, create a new repo on GitHub and name it "hello-blockstack":

> https://github.com/new

After that, add the remote repo to your local git repo. Make sure to fill in your username:

```bash
git remote add origin git@github.com:YOUR_USERNAME_HERE/hello-blockstack.git
```

Last, push all of the code to the master branch of the remote repo:

```
git push origin master
```

You're done! You just built your first Blockstack app and shipped the code.

You're well on your way to becoming a Blockstack app legend.
