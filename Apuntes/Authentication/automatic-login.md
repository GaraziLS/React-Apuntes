## Automatically Checking if a User is Logged In in React

If we login the login status will change, but if we refresh the page the status will say ```NOT_LOGGED_IN``` again. We want to mantain the state. 

In the **app.js** file we're going to add a couple of methods, the first one being ```CheckLoginStatus()```. We'll need axios as well. Inside of the function we'll make a return statement and call axios with a get request. As the argument we'll type the endpoint and an object containing ```withCredentials: true```.

```
 CheckLoginStatus() {
    return axios.get("https://api.devcamp.space/logged_in", {withCredentials: true})
  }
```

Axios works with promises, so we'll have to call the .then.

```
handleUnSuccessfulLogin() {
    this.setState({
      loggedInStatus: "NOT_LOGGED_IN"
    });
  }
  
  CheckLoginStatus() {
    return axios.get("https://api.devcamp.space/logged_in", {
    withCredentials: true
    }).then(response => {
      console.log("logged_in_return", response)
    })
  }

  componentDidMount() {
    this.CheckLoginStatus()
  }

  render() {...}
  ```

  The object that goes into the console when we printed it out tells us that the login session is false. In the auth page we'll login, and if we come back to the auth page now the object returns true despite the status displaying NOT_LOGGED_IN.

  We will now create a conditional inside the promise. We'll remove the console log and grab the data, store it in variables including the state, and then implement a conditional. 

  ```
  CheckLoginStatus() {
    return axios.get("https://api.devcamp.space/logged_in", {
    withCredentials: true
    }).then(response => {
      const loggedIn = response.data.logged_in;
      const loggedInStatus = this.state.loggedInStatus
    })
  }
  ```

  The response is the response, and the **data.logged_in** can be found in the console. The **loggedInStatus** part refers to the initial state. Now with all of this inside a variable we can do the consitionals. If the status is logged in we'll return data, if not, we'll update the state. If we're not logged in and the status says LOGGED_IN we'll update the state to log out as well.

```
CheckLoginStatus() {
    return axios.get("https://api.devcamp.space/logged_in", {
    withCredentials: true
    }).then(response => {
      const loggedIn = response.data.logged_in;
      const loggedInStatus = this.state.loggedInStatus;

      if (loggedIn && loggedInStatus === "LOGGED_IN") {
        return loggedIn;
      } else if (loggedIn && loggedInStatus === "NOT_LOGGED_IN") {
        this.setState({
          loggedInStatus: "LOGGED_IN"
        })
      } else if (!loggedIn && loggedInStatus === "LOGGED_IN") {
        this.setState({
          loggedInStatus: "NOT_LOGGED_IN"
        })
    }
  })
}
```
> ! refers to the opposite. so !loggedIn == false.

Just in case, we'll add a catch as well:

```
.catch(error => {
    console.log("ERROR FOUND", error);
  })
```
