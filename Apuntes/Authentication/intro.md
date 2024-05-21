## What's authentication?

Authentication is a way of identifying yourself. It often requires a user to first sign up and then log in. In a portfolio only the owner should log in, so it won't be visibe to other users and if someone tries to enter an error is going to pop up. First we're going to create the component inside the *app.js* file, as well as importing in from the app and creating its route:

```
import Auth from "./pages/auth"

<Route exact path="/auth" component={Auth}/>

export default class Auth extends Component {
    constructor() {
        super();

}
    render() {
        return (
        <div>
            Auth...
        </div>
        );
    };
}

```

## Creating the login component

We already have an auth page, now we're gonna create the login component, which is inside src > components > Auth > login.js.

```
import React, { Component } from 'react';

export default class Login extends Component {
    render() {
        return (
        <div>
            <h1>Form goes here</h1>
            <div>Button</div>
        </div>
        );
    };
}
```

Inside of that we're going to create the skeleton for the form:

```
return (
        <div>
            <h1>LOGIN TO ACCESS YOUR DASHBOARD</h1>
                <form>
                    <input type="email"/>
                    <input type="password"/>
                </form>
            <div>Button</div>
        </div>
        );
```

In React, as the user is typing in in a form, the state gets updated. When the user hits submit a handler will take care of what do you as a developer want to happen.

Inside the class we'll add states, that will start as empty strings:

```
export default class Login extends Component {
    Constructor(props) {
        super(props);

        this.state = {
            email: "",
            password: ""
        }
    }
}
```
Now we need to add an event handler to the form:

```
<form onSubmit={}>
```

Inside the curly brackets we're going to pass a function **WITH NO PARENTHESES** to avoid errors:

```
<form onSubmit={this.handleSubmit}>
```

We'll now add a name to the form (can be renamed), and a placeholder that will add a default message to the fields, and a value that must match up the state. Finally, we'll add the event handler:

```
<form onSubmit={this.handleSubmit}>
    <input 
    type="email"
    name="email"
    placeholder="Type in your email"
    value={this.state.email}
    onChange={this.handleChange}
    />
```

Repeat the process for the password:

```
<input 
     type="password"
     name="password"
     placeholder="Type in your password"
     value={this.state.password}
     onChange={this.handleChange}
    />

```

Now we'll work with the two handlers. Remember to bind the functions to the component:

```
export default class Login extends Component {
  constructor(props) {
    super(props);

    this.state = {
      email: "",
      password: ""
    };

    this.handleChange = this.handleChange.bind(this)
    this.handleSubmit = this.handleSubmit.bind(this)
  }}
  ```

  Now, if you write in the fields no text will render. Everytime we're typing we are setting the value of *this.state.email*. So, this value is overriding everything, which means that because we're not updating the value, it is always remaining just an empty string.

  To fix this we'll come to the handlers and set state inside them:

  We can dynamically change and pick the name that we're getting. So, the event has what is called a target. So, I can say *event.target.name*, and what this is gonna do is it's going to pick up the event that we got, this change event it's gonna pick up a target and then that target has attributes.

  In the chrome console, in the SyntheticEvent object, there's a target object. If, with the debugger on, we look for the event (by typing it), we'll see that the target has a name value. 

  But we first have to wrap up the event.target.name in brackets.

  > Whenever you create a normal JavaScript object, and pass a key-value pair, Well, if you're passing a dynamic name, so if you're actually getting this from a JavaScript primitive like what we have right here with this object, then you cannot just say the key, you have to wrap it up in brackets.

  ```
  handleChange(event){
    this.setState({
        [event.target.name]: event.target.value
    });
  }
  ```

  Now the fields are being updated.