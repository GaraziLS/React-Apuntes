# Deep Dive: Component Lifecycle in React

**Mounting**: Here, we're picking out what kinds of data and processes need to occur when that component is going to load, with all the processes that will occur when someone navigates to that component. That's the mounting process and it's that first stage kind of like when we're going to the drive-thru and we're starting to pick out what we want to order.

**Updating**: If when we order something there isn't that kind of item we can change our minds and place enother order. In code terms, that would be like filling forms or something. If only one component needs to be updated out of eight, we can update just one.

**Unmounting**: Once we place our order we pay, receive food and go away. That's the *unmounting* process.

We´ll start by adding routing. This will allow to hide (unmount) components when others are loaded. Simply put it will be a way of jumping between links.

Briefly put, to create a link navigation system in react we need:

1. To go to the entrypoint, go to src > index.js, and wrap the app component inside of a browser router.

2. Create a navlink page that will wrap all the links that will point to different "pages". They will point to routes.

3. To switch between links, create a page that will dsplay the content. Use a switch tag and specify the path.

4. Export components to the app file.

To do so, go to components > src and create a file (name it however you want).

After exporting this component to the App file, go to the entry point (src > index.js), and wrap the app component inside of a browser router (and import the BrowserRouter):

```
import React from 'react';
import ReactDOM from 'react-dom';
import {BrowserRouter, Route, Link} from "react-router-dom";

import App from './components/app';

ReactDOM.render(
  <BrowserRouter> // <---- Wrapper
  <App />
  </BrowserRouter>, // <---- Wrapper
  document.querySelector('.app-wrapper'));
  ```
  This will show and hide one component and hide the others, allowing us to jump between links.

  Now we'll create another file inside the components, *navigaton.js*, where all of the links will be:

  ```
  import React, { Component } from 'react';

import { NavLink } from 'react-router-dom';

export default function()  {
        return (
        <div>
            <NavLink exact to= "/">
            Discussion
            </NavLink>
        </div>
        );
    };
```

The NavLink component is provided by React. The exact prop specifies the route to which the link will point (the slash indicates a homepage) and must be wrapped by NavLink tags. This way we can jump between components.

To make the jumps, we'll type:

```
import React, { Component } from 'react';
import { Switch, Route } from 'react-router-dom';
```
And then we'll import the workflow, rules and discussion pages here as well.

To make the switches between links, we'll use:

```
export default function() {
        return (
        <div>
            <Switch>
                <Route exact path="/" component={Discussion}/>
                <Route exact path="/rules" component={Rules}/>
                <Route exact path="/lifecycle_workflow" component={Workflow}/>
            </Switch>
        </div>
        );
    };
```

Where the switch tag encapsulates the routes.

Now we have to import the page content and the navlink files from the app.

```
import React, { Component } from 'react';

import Navigation from './navigation';
import PageContent from './page-content';

export default class App extends Component {
  render() {
    return (
    <div>
         <Navigation/>
         <PageContent/>
    </div>
    );
  };
}
```
