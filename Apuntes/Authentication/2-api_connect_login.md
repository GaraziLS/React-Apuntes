# Creating an authentication session in React

Now we'll actually login by connecting to the API. At the very top of the login component we'll import axios:

```
import axios from "axios";
```

And inside the handleSubmit we're going to remove the console.log statement and we're going to call axios to make a post request:

> When we brought in the portfolio items, we used the GET method, but now because we're actually pushing something up and we want to create something, namely, we want to create what is called a session. We're creating the ability to create a session on the server so the server is going to send back a cookie, it's gonna store the cookie directly in the browser, and that is what is going to be able to be used to make sure that we have the ability to login or if we do not have that ability.

Inside of post, the very first argument is the URL. So the URL that we're going to be using for this is gonna be https://api.devcamp.space/sessions. We now have to pass an object (client object), but this is specific to this API. This object will store the password and the email.

That is what is going to give us the ability to send up a client object to sessions to see if we are authorized or not. The API is gonna take care of all these.

```
handleSubmit(event) {
    axios.post(
        "https://api.devcamp.space/sessions",
        {
          client: {
            email: this.state.email,
            password: this.state.password
          }
        },
      )
}
```

Now we're going to pass another object:

```
{withCredentials: true}
```
With this the server will check if we're authorised or not.

Axios works with promises because the response isn't immediate. With this, we're able to console out the login.

```
handleSubmit(event) {
    axios.post(
        "https://api.devcamp.space/sessions",
        {
          client: {
            email: this.state.email,
            password: this.state.password
          }
        },
        {withCredentials: true}
      ).then(response => {
        console.log("response", response)
      })
    event.preventDefault();
  }
  ```
## Handling sign in error in React

What happens when you type credentials wrong? We're going to handle that.

Right now, nothing happens, but the data status throws a 401 error (not authorised). We want to catch that error, and sone where the API is incorrectly structured (as in when you type a wrong route) or down (which would throw a 404 error).

In the login.js component, we'll update the state with an error text:

```
this.state = {
      email: "",
      password: "",
      errorText: ""
    };
```

```
<h1>LOGIN TO ACCESS YOUR DASHBOARD</h1>

    <div>{this.state.errorText}</div>
```
To start catching errors we gust go tho the end of the *.then* and call catch. This will throw an error when the API is down, for example.

```
handleSubmit(event) {
    axios.post(
        "https://api.devcamp.space/sessions",
        {
          client: {
            email: this.state.email,
            password: this.state.password
          }
        },
        {withCredentials: true}
      ).then(response => {
        console.log("response", response);
      }).catch(error => {
          console.log("error found", error);
          this.setState({
            errorText: "An error occurred"
          })
      });
    event.preventDefault();
  }
  ```

  However this will only give an error number. It's better to get a conditional:

  ```
  .then(response => {
        if (response.data.status === "created") {
          console.log("You can come in...");
        } else {
          this.setState({
            errorText: "Wrong email or password"
          });
        }
      }).catch(error => {
            this.setState({
                errorText: 'An error ocurred.'
            })
        });
        this.props.handleUnSuccessfulAuth();

    event.preventDefault();
  
```

If the user is typing and they get the correct values, the errorText will go empty again, thus reseting the warning.

```
handleChange(event){
    this.setState({
        [event.target.name]: event.target.value,
        errorText: ""
    });
  }
```

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

## Deep dive: authentication

We'll use postman to mimic what APIs do. After setting up a new devcamp space account. Then, we'll go to postman, type the session url in a post request, go to body > raw and select json, and start typng.

![Auth postman 1](https://s3-us-west-2.amazonaws.com/images-devcamp/Dissecting+React+JS/React+JS+Authentication/Deep+Dive%3A+Authentication+%23+2398/image11.png)

We're going to create an object now:

```
{
    "client": {
        "email": "demoaccount@devcamp.com",
        "password": "asdfasdf"
    }
}
```

and Postman will return ```"status": "created"```.

![Auth postman 2](https://s3-us-west-2.amazonaws.com/images-devcamp/Dissecting+React+JS/React+JS+Authentication/Deep+Dive%3A+Authentication+%23+2398/image12.png)

We now need to tell the browser, or I should say the application, the API, needs to tell the browser that the user is now authenticated, and that is where the cookies come in.

The cookies are automatically placed in the browser, allowing us to stay authenticated. If those credentials are false, you don't log in.
