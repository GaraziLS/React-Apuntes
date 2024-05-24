## Logout handler

Now we're going to create a way to log out. In the **app.js** file, we'll build another handler (don't forget to bind it).

```
handleSuccessfulLogout() {
    this.setState({
      loggedInStatus: "NOT_LOGGED_IN"
    });
  }
```

Now we'll call it inside the navigation container:

```
<NavigationContainer 
     loggedInStatus={this.state.loggedInStatus}
     handleSuccessfulLogout={this.handleSuccessfulLogout}
     />

```

## Finishing log out and high order components

In the **NavigationContainer** file, we're going to import axios because we're going to communicate with the logout endpoint. *Above the return statement and below the NavigationContainer *function we're going to create a new function called handleSignOut. Inside it, we're going to call axios and from inside it the .delete verb that takes an API endpoint as an argument, as well as the WithCredentials item:

```
const handleSignOut = () => {
    axios.delete("https://api.devcamp.space/logout", {withCredentials: true})
  }
```

>  So far we've used GET to pull data down or to run queries. POST is what we use when we want to create something. DELETE is kind of self-explanatory, it's when you want to delete something, in this case the session. Now we must update the promise (still inside the function) with what should be done after the logout. In this case it will be a conditional.

```
const handleSignOut = () => {
    axios.delete("https://api.devcamp.space/logout", {withCredentials: true}).then(response => {
      if (response.status === 200) {
        props.history.push("/")
      }
    })
  }
  ```

  > The 200 means that the API worked, and we're redirected to the root route in that case. From that we'll call the handler of the logout, and use a return statement to get the data, as well as a catch statement.

  ```
  const handleSignOut = () => {
    axios.delete("https://api.devcamp.space/logout", {withCredentials: true}).then(response => {
      if (response.status === 200) {
        props.history.push("/")
        props.handleSuccessfulLogout();
      }
      return response.data;
    }).catch(error => {
      console.log("Error signing out", error)
    })
  }
```

Now we're going to call the function from the JSX, with a conditional to check if we're logged or not, just to display the correct links.

> We don't use the .this because the handler is called inside a functional component.

```
{props.loggedInStatus === "LOGGED_IN" ? <a onClick={handleSignOut}>Sign out</a> : null}
```

If we click on that an error will pop up (we don't have access to the history). To fix that, we'll use High Order Components (*they're old and nowdays aren't used*).

> A higher-order component is a function that takes a component and returns a new component.

So we're going to import a HOC:

```
import {withRouter} from "react-router"
```

And we'll call it in the export default while passing the NavigationComponent as an argument:

```
export default withRouter(NavigationComponent);
```

This will have access to the props.history, and now we're able to log out.