## Render props

Now we will start building the entire authentication process starting at the app component level, so that we can manage authentication in every page of our entire app (when logged in more links will appear, when logged out those disappear and the NoMatch route kicks in).

So in the **app.js** file, right after the class creation line, we'll add a constructor that takes props, as well as the initial state:

```
export default class App extends Component {
  constructor(props) {
    super(props);

    this.state = {
      loggedInStatus: "NOT_LOGGED_IN"
    }
  }
}
```

> The caps are a convention, as well as the names. We're not using booleans because JS can be a little bit confusing with conditionals that check for boolean values. In short, **don't use booleans**.

From there we'll build some handlers:

```
 handleSuccessfulLogin() {
    this.setState({
      loggedInStatus: "LOGGED_IN"
    })
  }

  handleUnSuccessfulLogin() {
    this.setState({
      loggedInStatus: "NOT_LOGGED_IN"
    })
  }
  ```

In order to not have any errors, we also have to bind these handlers to the constructor and bind them to this. So at the very top of the constructor and right after the state, type ```this.handleSuccessfulLogin = this.handleSuccessfulLogin.bind(this)``` and the same for the other handler.

```
export default class App extends Component {
  constructor(props) {
    super(props);

    this.state = {
      loggedInStatus: "NOT_LOGGED_IN"
    }
  

    this.handleSuccessfulLogin = this.handleSuccessfulLogin.bind(this)
    this.handleUnSuccessfulLogin = this.handleUnSuccessfulLogin.bind(this)
  }
  
  handleSuccessfulLogin() {
    this.setState({
      loggedInStatus: "LOGGED_IN"
    });
  }

  handleUnSuccessfulLogin() {
    this.setState({
      loggedInStatus: "NOT_LOGGED_IN"
    });
  }
  
  render() {...}
}
```

  We're going to use these methods via render props. A render prop is a prop passed to a component but that's able to communicate with the render method.

  > Now the steps that we will follow are in the **react-router documentation**.

  In the route section of the **app.js** file we'll delete the component to add a render prop, and then pass in props (that takes a function as its argument. Then we're going to pass the actual component as normally and from there we're going to add more props).

  > Props is underscored to fix the declared but never used issue.

  ```
  <Route 
    exact path="/auth" 
    render={_props => (
      <Auth/>
    )}
    />
```

From the inline Auth component we're going to pass props, but first we must use the spread operator **{...}** and pass in props. From there we'll add the handlers.

```
<Route 
    exact path="/auth" 
    render={_props => (
      <Auth
        {..._props}
        handleSuccessfulLogin={this.handleSuccessfulLogin}
        handleUnSuccessfulLogin={this.handleUnSuccessfulLogin}
      />
    )}
    />
```

> Instead of passing in a regular prop, like path or something like that, we're also passing in a render prop. So we're overwriting the render process. Here, we're getting access to the props, which is the first argument in this function. Inside of this render, I want you to call the Auth component and that's what I want you to render. I want you to allow it to have access to all the props **(spread method)** it would have been given anyway, like Auth and those kinds of things. But then also, I want to override it and pass in my own set of functions, my own types of handlers.


## Updating a Parent Component's State in React from a Child Component

We already saw how to pass props from a parent to a child (via props). No we'll do the opposite, by passing things from children to parents.

The login component communicates with the outside service, and it logs us in and creates the session, stores the cookie in our browser, and performs those kinds of tasks.

But it's not really doing anything as far as following the standard kind of usage. We would expect with a login component that you would log in and then it would redirect us to the homepage or some other page and then give us some additional access points, but for right now, it's not doing that, so in this guide, we are going to do that.

We're gonna start at the app component, we've already added our render prop, and we've made it possible for all of that to work. Then from there, we are going to go to the auth component. We're gonna have it accept the props, work with it, and then pass props to the login component.

**To start with this** we'll add an h2 in the homepage (**app.js,** below the *NavigationContainer*) that keeps track of the status of the login just for testing purposes.

```
 <NavigationContainer/>
    <h2>{this.state.loggedInStatus}</h2>
```

Now we're going to the **auth.js file**. This component will communicate with the *app.js* file but it's also going to send data back and forth. We'll start by creating a construnctor and a couple of handlers that will update the props they receive (***handleSuccessfulLogin*** and ***handleUnsuccessfulLogin*** , and then they are gonna be the props that are passed down to the login component). These **new handlers** are gonna be ***handleSuccessfulAuth and handleUnSuccessfulAuth.***

  > Binding must be done every time we're passing a method down through props.

 So the new handlers receive props (just like regular props) and a function, in this case the other set of Login handlers:

```
export default class Auth extends Component {
  constructor(props) {
    super(props);

    this.handleSuccessfulAuth = this.handleSuccessfulAuth.bind(this);
    this.handleUnSuccessfulAuth = this.handleUnSuccessfulAuth.bind(this);
  }

  handleSuccessfulAuth() {
    this.props.handleSuccessfulLogin()
  }

   handleUnSuccessfulAuth() {
    this.props.handleUnSuccessfulLogin();
}
}
  ```

 Now we're going to perform the redirection to the homepage.

 ```
 handleSuccessfulAuth() {
    this.props.handleSuccessfulLogin();
    this.props.history.push("/")
  }
  ```

  The history portion what's doing is it's looking into the history of our browsing, not the browser history in terms of any other websites that you go to, we don't have any access to that. This gives us access to the history from the first time that a user accessed the site or accessed the application. Where did they navigate to? What other routes did they hit? Each time they hit a new route, it adds onto the history, and so what we can say is *this.props.history*, and then push, and then simply as a string, a backslash. This will redirect to the homepage.

  Now we have to call this from the **inline login component** (in the **Auth.js** file) and it will communicate with the app component.

  ```
  <Login 
    handleSuccessfulAuth={this.handleSuccessfulAuth}
    handleUnSuccessfulAuth={this.handleUnSuccessfulAuth}/>
```

Now in the login.js component we need to update the promise just as we pass on props, but passing the new handlers instead:

```
.then(response => {
        if (response.data.status === "created") {
          console.log("You can come in...");
          this.props.handleSuccessfulAuth()
        } else {
          this.setState({
            errorText: "Wrong email or password"
          });
        }
      })
      .catch(error => {
        this.setState({
          errorText: "An error occurred"
        });
        this.props.handleUnSuccessfulAuth();
      });
```
