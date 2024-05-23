## How to Show and Hide Links When Logged In

When logging in we don't want to have the log in button on screen. Or that we only want specific links to show when we're logged in. We'll first show and hide links and then work with the routes themselves.

In the **NavigationContainer.js** file we're going to move te blog link right after the const part:

```
const NavigationComponent = _props => {
  <div className="nav-link-wrapper">
          <NavLink to="/blog" activeClassName="nav-link-active">
            Blog
          </NavLink>
        </div>
}
```

From here we'll make a function right below the const, and paste that blog route in:

```
const NavigationComponent = _props => {
  const dynamicLink = (route, linkText) => {
    return (
      <div className="nav-link-wrapper">
          <NavLink to="/blog" activeClassName="nav-link-active">
            Blog
          </NavLink>
        </div>
    )
  }
```

And now we're going to call the function where the blog route used to be, and this will still work:

```
{dynamicLink("/blog", "Blog")}
```

In the **app.js** component, in the NavigationContainer component, we'll add a prop:

```
<NavigationContainer loggedInStatus={this.state.loggedInStatus}/>
```

Now inside of the dynamicLink function we'll call that prop and pass a ternary operator:

```
{props.loggedInStatus === "LOGGED_IN" ? dynamicLink("/blog", "Blog") : null}
```

Now when we're logged in we'll see the Blog link, and when we're not logged in not, but the route will stil exist. In the next guide we're going to pull out the guide if the user isn't logged in.


