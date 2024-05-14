## Intro to routing

From a document perspective, a react app is a single page aplication. Redirection to different "pages" (represented by the buttons) is done with route paths that actually point to components, despite users will think that there are multiple pages.

> You can add aliases to the imports y calling the library name and then adding as *alias*. ```import {BrowserRouter as  Router}```

We've been using self-closing tags till now, but in react you can also wrap code inside components (in react, inside return statements everything must be wrapped inside a single div).

```
export default class App extends Component {
  render() {
    return (
      <div className='app'>
        <Router>
          <div>
          <NavigationContainer/>
          <Switch>
            
          </Switch>
          </div>
        
        </Router>
        (...))}}
```

Inside the switch we'll call the Route component, which will have a path ponting to a slash. This will represent our homepage (also known as route root), but we'll have to create the page itself. To do this we'll create a pages folder, and then we'll export those components to *app.js*.

```
import React, { Component } from 'react';
import moment from "moment";
import {BrowserRouter as  Router, Switch, Route } from "react-router-dom"


import NavigationContainer from './Navigation/navigation-container';
import Home from "./pages/homepage";
import About from "./pages/about";
import Contact from "./pages/contact";
import Blog from "./pages/blog";
import PortfolioDetail from "./portfolio-components/portfolio-detail"
import NoMatch from "./pages/no-match"

export default class App extends Component {
render() {
    return (
      <div className='app'>
        <Router>
          <div>
            <NavigationContainer/>
            
            <Switch>
                <Route exact path="/" component={Home}/>
                <Route path="/about-me" component={About}/>
            </Switch>
          </div>
        
        </Router>
    )}
}
```

> The route paths can be named like you want, what matters is the component itself (which is imported above).

The route component is already imported above, and it takes props such as path.

> The exact param comes into play when you have multiple paths that have similar names: For example, imagine we had a Users component that displayed a list of users. We also have a CreateUser component that is used to create users. The url for CreateUsers should be nested under Users. So our setup could look something like this:

```
<Switch>
  <Route path="/users" component={Users} />
  <Route path="/users/create" component={CreateUser} />
</Switch>
```

> Now the problem here, when we go to http://app.com/users the router will go through all of our defined routes and return the FIRST match it finds. So in this case, it would find the Users route first and then return it. All good.

> But, if we went to http://app.com/users/create, it would again go through all of our defined routes and return the FIRST match it finds. React router does partial matching, so /users partially matches /users/create, so it would incorrectly return the Users route again!

> The exact param disables the partial matching for a route and makes sure that it only returns the route if the path is an EXACT match to the current url.

> So in this case, we should add exact to our Users route so that it will only match on /users:

```
<Switch>
  <Route exact path="/users" component={Users} />
  <Route path="/users/create" component={CreateUser} />
</Switch>
```
**To avoid problems make all the routes exact.**

## Making actual redrections to the pages

To actually redirect users to pages we need to use Navlinks in the navigation container, in this case.

The syntax for NavLinks is pretty similar to routes. They're called inside the render function and have a path too:

```
 render() {
        return (
        <div>
            <NavLink exact to="/">Home</NavLink>
             <button>Home</button>
(...))}
```

> The navigation container has the Navlinks themselves. The routes, however, are specified in app.js.

> If we used a regular < a > tag there would be issues.

## Navlinks' active status

If we look into the Chrome dev tools we'll see that React is creating HTML on the fly. When clicking on a link, it will get the "active" class. This will allow to give CSS styles. If you want to override that you can pass an *activeClassName* prop and name it however you want.

![active class navlinks](https://s3-us-west-2.amazonaws.com/images-devcamp/Dissecting+React+JS/React+Router/Guide+to+Working+with+NavLink+Active+Styles+%23+2360/image11.png)

## The Link component

```
import {Link} from 'react-router-dom';

export default function() {
    return(
    <div>
        <h2>Blog</h2>
        <div>
            <Link to="/about-me">Read more about me</Link>
        </div>
    </div>
);
}
```

The Link component is more specific than vanilla < a > links. When clicking on it it doesn't reload, nor does get the active class like Navlinks. (Could be usable as dropdown menus?)

> PortfolioContainer got moved from app.js to the homepage component, so now it will have its own separate page.

## Access URL values im React

We're going to access data inside the items too in order to get more information about a specified item.

To do so, inside of the app.js we're going to add another route.

```
<Route exact path="/portfolio/:slug" component={PortfolioItem}/>
```

> :slug is the naming convention for custom URL endpoints. Another one is permalink.

With all of that and after creating the new component we can type the url endpoint into the bar and we'll be redirected to the portfolio detail item. How do we access to the data though? 'cause the APIs need to know what data to bring back.

To access data, props are the key. Inside the *portfolio-detail.js* component we're going to pass props to the function, and then we're going to type ```{props.match.params.slug}``` to get the custom endpoint.

```
export default function(props) {
    return(
    <div>
        <h2>Portfolio Detail for {props.match.params.slug}</h2>
    </div>
    );
   }
   ```

This will get us the url custom endpoint, but we also want a link to those pages. Before doing that though, we'll add more data to the data object of the *portfolio-container.js* to mimic what APIs do.

```
this.state = {
  pageTitle: "Welcome to my portfolio",
   data: [
      {title: "First", category: "eCommerce", slug: "first"},
       {title: "Second", category: "eCommerce", slug: "second"},
       {title: "third", category: "Personal", slug: "third"}, 
       {title: "fourth", category: "Hobby", slug: "fourth"}
      ],
}
```

We'll also update the PorfolioItems()' components to add the slug as a prop and to the _item:

```
 portfolioItems() {
    return this.state.data.map(item => {
      return (
        <PortfolioItem title={item.title} url={"google.com"} slug={item.slug} />
      );
    });
  }

```

In the portfolio-item.js component, we'll now add a link and use string interpolation in the path to add props.slug (the object key).

```
export default function(props) {
    return(
        <div><h3>{props.title}</h3>
        <h4>{props.url}</h4>

        <Link to={`/portfolio/${props.slug}`}>Link</Link>
        </div>
    );
}

```

> (We add curly brackets because this is JS code)

We now have functional dynamic links that redirect to each component.

## Implementing a catch all route with react router

What happens when a route doesn't exist? In some languages/frameworks that would throw an error in the server, but in React we only have one page, and a wrong route still shows the page. Some feedback would be greate to indicate that that page doesn't exist, so we'll create another component called NoMatch, and in the route section we'll create another one.

When a route doesn't have a path that will be the one that's picked up:

```
<Route component={NoMatch}></Route>
```
> Never put this kind of routes at the very top of the switch statement, because this would be the route captured instead. It's like saying *No matches found, pick this one instead as default*. It's like an else.



