## Components

To use components you first need to import react at the very top. Components are not mandatory. 

```
import React, { Component } from 'react';
```
> ### A note on curly brackets
>
> Curly brackets are needed when you are specifically picking out a library that has not been exported by default.
> 
> In the app.js (inside src) there's a line of *export default class App*. What this means is that some other file (Bootstrap) is importing the file:
> 
> ```
> import App from "./components/app";
> ```
> We do not use the curly brackets there and the reason for that is, because whenever you have an export default, what that means is that you're just pulling in the file, and you're treating the entire file like one library and then you can call it whatever you want. 
> 
> Whenever you're using these curly brackets, that would be kind of akin to saying, "I want to simply export the ClassApp, not default." Then you have to be very specific, and you have to put in curly braces, and you have to name it.

If you're using a component you need a render() method. This method has a return statement that returns what's shown on the page.

```
export default class App extends Component {
  render() {
    return (
      <div className='app'>
        <h1>Some content</h1>
        <h2>{moment().format('MMMM Do YYYY, h:mm:ss a')}</h2>
      </div>
    );
  }
}
```

## Create components

* Inside the src > components folder, create a new file.

* Then, import react (and non-defaulted components) from its module, and create a class with *export default class + the Class name*. This class (highlighted in green) extends (creates a child) a Component. Inside of there, between curly brackets, there's a render method that has a return statement that contains, between (), JSX (looks like HTML). This return statement is what's rendered on the page after calling it in the app.

```
import React, {Component} from "react";

export default class PortfolioContainer extends Component {
    render() {
        return (
            <div>
                <h2>Portfolio Container</h2>
            </div>
        )
    }
}
```

* To render this on the page it's necessary to call it from the app.js file, but you first need to import it.


* It's a best practice to import things from your own code separated from the libraries imports. This is done by importing the component's name and by specifying its path. Remember to use the export method inside the component that will be imported.

```
import React, { Component } from 'react';
import moment from "moment"

import PortfolioContainer from './portfolio-components/portfolio-container';

export default class App extends Component {
  render() {
    return (
      <div className='app'>
        <h1>Some Content</h1>
        <PortfolioContainer/>
        <h2>{moment().format('MMMM Do YYYY, h:mm:ss a')}</h2>
      </div>
    );
  }
}
```
* After importing a component you call it in the app. To do so, type its name between < />

## Class vs Functional (also presentational) components

Class components are class-based components, like the ones above. Functional (or presentational) components, on the other hand, are function-based and look like this:

```
import React from "react";

export default function() {
    return(
        <div><h3>Portfolio item</h3></div>
    )
}
```
Now we're gonna export it to the container, which in turn is imported from the app file:

```
import React, {Component} from "react";

import PortfolioItem from "./portfolio-item"

export default class PortfolioContainer extends Component {
    render() {
        return (
            <div>
                <h2>Portfolio Container</h2>
                <PortfolioItem/>
            </div>
        )
    }
}
```

A functional component, despite being written with less code, is less functional. To work with states and lifecycles class components are needed. To simply render content, functional components are more than enough.

Functional components such as the portfolio item will only store data, not images, links, etc. That's for the class components.

### Constructors inside class components

Constructors allow to automate processes and can only be used inside class components. They allow to give a component an initial state, and look like this:

```
export default class PortfolioContainer extends Component {
    constructor() {
        super(); 
            console.log("All the portfolio items have been loaded")
    }
    render() {
        return (
            <div>
                <h2>Portfolio Container</h2>
                <PortfolioItem/>
            </div>
        )
    }
}
```
Classes extend from components, so the super method allows to call the parent class.

### Custom functions in React and looping

Looping through data and then rendering it is quite common, to avoid hardcoding and to generate things dynamically.

To do this in React we'll use the map function, so we might have this: data, with the map function, that has a key and an arrow function inside it as arguments:

> The map function always returns something. 
> Item is prefixed with an undercore because otherwise it is never read despite being declared.

```
portfolioItems() {
        const data = ["First", "Second", "Third"];
        return data.map(_item => {
            return <PortfolioItem/>; })
    }
```

Yes, in react you can return components inside functions.

In order to render this we need to use curly brackets, because when you want to slide some JavaScript code into your render function here, you need to use curly brackets, and we are rendering the function that has a component inside it:

```
 portfolioItems() {
        const data = ["First", "Second", "Third"];
        return data.map(_item => {
            return <PortfolioItem/>;
        })
    }

    render() {
        return (
            <div>
                <h2>Portfolio Container</h2>
                <div>{portfolioItems()}</div> // <---- Functional component called via function with curly brackets
            </div>
        )
    }
```

Now that would return a component, but if we do this

```
portfolioItems() {
        const data = ["First", "Second", "Third"];
        return data.map(_item => {
            return <h1>{_item}</h1>;
        })
    }

```

We get the data looped.

### Props (properties)

Props (short for properties) are the way that you can pass data from one parent component to a child component. 
 The very first thing that we're going to do when you're passing in a prop is the syntax for that is to define it directly in line with the component. 

 ```
 portfolioItems() {
        const data = ["First", "Second", "Third"];
        return data.map(_item => {
            return <PortfolioItem title={_item}/>; // component and prop with the _item inside
        })
    }
```

We now need the component itself to be able to render the prop in the page. To do this we must go to the component and inside the function pass the prop element via the reserved word props:

```
export default function(props) {
    return(
        <div><h3>{props}</h3></div>
    )
}
```

However this will throw an error because props is an object and objects aren't valid as react children. We need to call the prop itself (the tag, not the content) inside the component too:

```
export default function(props) {
    return(
        <div><h3>{props.title}</h3>
        <h4>{props.url}</h4></div>
    )
}
```

And this will return each item again, and the specified url: 

```
First
www.google.es
Second
www.google.es
Third
www.google.es
```

### JSX

This compiler converts data into something all browsers can handle. When creating a component in react, this is what JSX does:

![jsx behind scenes](https://s3-us-west-2.amazonaws.com/images-devcamp/Dissecting+React+JS/React+JS+fundamentals/How+JSX+Works+in+React+%23+2352/image11.png)

![jsx behind scenes](https://s3-us-west-2.amazonaws.com/images-devcamp/Dissecting+React+JS/React+JS+fundamentals/How+JSX+Works+in+React+%23+2352/image12.png)