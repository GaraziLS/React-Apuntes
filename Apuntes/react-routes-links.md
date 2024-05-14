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
```

> The route paths can be named like you want, what matters is the component itself.

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

> If we used a regular <a> tag there would be issues.

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

The Link component is more specific than vanilla <a> links. When clicking on it it doesn't reload, nor does get the active class like Navlinks. (Could be usable as dropdown menus?)

> PortfolioContainer got moved from app.js to the homepage component, so now it will have its own separate page.