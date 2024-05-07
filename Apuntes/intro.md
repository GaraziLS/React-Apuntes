## Adding, removng and managing node modules

* ***Manual addition***: type the module name & version in node_modules > package.json > dependencies **in alphabetical order** followed by a comma. Then, run npm install. It will add into the system the new additions of package.json

```
"moment": "^2.30.1",
```
* ***Automatic addition***: Just copy the command line from the library's documentarion into the terminal.

### Removing modules

To remove dependencies, first remove them from package.json and run npm install. This will correct the dependency list and remove what's removed. Alternatively, delete the node-modules and re-run npm install. ***Never*** make changes to the node_modules folder directly.

Note: install npm first with **npm install** in the project you're in. To run npm server, 
```
npm run start *or* npm start
```

Note 2: Remember to import modules before writing code: *import x from "x"* (if it's in the node modules library) or specify path. To call libraries, write between {}

## React folder overview: src

* ***Actions***: Has an index. What this is going to do is it's going to give us the ability to interact with (reduces do this as well) our Redux store. Redux gives us the ability to store all of our data in a single place and then to query it and then access it.

* ***Components***: This is a very key element of our entire application. Components are what make up a React application. It means that we have the ability to build an app one piece at a time. Instead of creating all of these pages the way you would with... that was kind of the older way that you'd build out applications. With systems like React or View, they are component based systems, which means that we put together and we compose all of these components and then that is what gives us the full application. 

And we're going to place that logic inside of this components directory. Now, the template that you have right here just has a single component right now called the App component. This is a very critical one for our application because all of our other components are going to be nested inside of this App component.

* ***Styles***: To place all the (s)css

* ***Bootstrap***: It's the program's entry point for the app, and also imports all the components, including the styles. The user's entry point is an html file. If you don't have a bootstrap.js file, or a vendor.js file that's perfectly fine.

Whenever you're building out your own applications, there are quite a few ways of implementing the structure. So with bootstrap right here, what we're using it for is to make this the start of the application. So this is kind of like the entry point for everything that we're going to be building out.

Now, moving on down the line after all those imports we have a main function. This is where we're starting up the React application, we're saying that we want to have React dom, which is saying that we are using React dom which means it's going to be in the browser.

The reason why we have the word dom here is because if you were wanting to build out a React native mobile application, you wouldn't be using ReactDOM, you'd be using a React native library. So when you see the word ReactDOM, that means that we're using an application, or we're going to be building an application that is going to be rendered in the browser, not on a smartphone.

Every React application you're going to see is going to have a single entry point, it's just a plain html file. You can see we have a class here called app wrapper. So the way that React works is it loads up this bootstrap.js file, it calls ReactDOM, it loads in all those libraries and then it looks for a class called app wrapper, then it attaches our files, it attaches all of our components directly into this html file.

```
import React from "react";
import ReactDOM from "react-dom";
import { Provider } from "react-redux";
import { createStore, applyMiddleware } from "redux";
import { BrowserRouter } from "react-router-dom";
import App from "./components/app";
import reducers from "./reducers";

const createStoreWithMiddleware = applyMiddleware()(createStore);

import "./style/main.scss";

function main() {
    ReactDOM.render(    // DOM de React
        // store de Redux
        <Provider store={createSoreWithMiddleware(reducers)}>
            <!-- para crear routes personalizados -->
            <BrowserRoute>
                <App />    <!-- componente principal -->
            </BrowserRoute>
        </Provider>,
        document.querySelector('.app-wrapper')
    );
}

document.addEventListener("DOMContentLoaded", main);
```

So technically, the entire application is in a single page. I know that may sound weird, especially because there are some React applications that are gigantic. You have Facebook, which is a React application and it has a million lines of code. Technically though, it's still one file.

* ***Vendors***: Uploads polyfills, files that browsers can't support themselves.

## React folder overview: static

* ***Assets***: contains images, etc. and a readme to store what kind of files must be uploaded.
* ***Favicon***: Little icon next to the page
* ***Index.html***: The entrypoint to the app by users, and the main file of the app.

## React folder overview: webpack

Composed by common.config.js (common configurations of the app in any type of environment, and that has entrypoints and allowed extensions), dev.config.js (contains configuration in the development environment), prod(uction) (contains configuration in the production environment) and postcss (contains postCSS, that transfoms CSS with js).

## Babel

Files with a dot before the name (.babelrc) are hidden files: files that won't be seen in the folder structure.
Whenever you see rc, the letters rc at the end of a filename, that is almost always a configuration file. 

Babel is a JS compiler, it converts modern JS to something the browser can understand.

## Env.js and overriding server ports

Allows to acces environment variables, and can override the behavior of local servers, changing the local entry ports (localhosts), for example.

## Package.json file

Contains info about the project, such as the name or the versions, and the description, and the command to start the program (you can build custom commands). It also contains the npm dependencies and the dev dependencies, which are required to work locally.

To write json (Javascript object notation, the type of file of package files) you write key-value pairs.

So it starts with a regular object just like this, and then you set up a key. This could be anything, so "anything" here would be the name of the key. Then use a colon, and then whatever the value is. So then you could say "something else for the value", just like that, and if you wanna add more key value pairs, then you give a comma and then you can just keep going down.

![json object](https://s3-us-west-2.amazonaws.com/images-devcamp/Dissecting+React+JS/React+Course+Introduction/Overview+of+the+package.json+File+in+a+React+Application+%23+2340/image13.png)

Make sure that your keys and your values all have double quotes. Now, in most programming languages, the difference between single or double quotes does not matter that much, in json it does, though. 

Then also make sure that if you come to the end of your list of items, just like we have right here, then make sure that you do not have a trailing comma or else you will get an error.

## Package-lock.json

It contains more info about the dependencies than in package.json, such as the exact version or more libraries. This file is automatically generated and updated, touching it manually could cause conflicts.

## Procfile, readme and server js files

Procfile is used when deploying to the web. The README is where you explain your code or give install instructions.
The server js file is used in production.



