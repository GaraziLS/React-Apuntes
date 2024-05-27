A manager enables the creation of new items directly from the app. The program will then upload it to the API. We will also be able to upload images directly from the app.

## Building a Secure Class Component for the Portfolio Manager Page

To start off, we'll create the portfolio manager container inside the src > pages folder. Now we'll import it from the app, add the route, and add it in the authorised links part:

```
// portfolio-manager.js

import React, { Component } from 'react';

export default class PortfolioManager extends Component {
    constructor() {
        super();

}
    render() {
        return (
        <div>
            <h1>Portfolio Manager</h1>
        </div>
        );
    };
}

// app.js

import PortfolioManager from './pages/portfolio-manager';

<Route exact path="/about-me" component={About}/>
<Route exact path="/contact" component={Contact}/>
<Route exact path="/blog" component={Blog}/>
{this.state.loggedInStatus === "LOGGED_IN" ? this.AuthorisedPages() : null}
<Route exact path="/portfolio/:slug" component={PortfolioDetail}/>
<Route component={NoMatch}></Route>

AuthorisedPages() {
    return [
      <Route exact path="/portfolio-manager" component={PortfolioManager}/>
    ]
  }

```

If we go to the portfolio manager page when logged out the page won't exist. Now we'll add the portfolio manager to the nav bar. To do that, in the **navigation-container.js** file type this:

```
<div className="nav-link-wrapper">
    <NavLink to="/blog" activeClassName="nav-link-active">
      Blog
    </NavLink>

```

Now we want to update the dynamic link to add the new routes:

```
{props.loggedInStatus === "LOGGED_IN" ? dynamicLink("/portfolio-manager", "Portfolio Manager") : null}
```

Now we'll change the dynamic link constant from this:

```
const dynamicLink = (route, linkText) => {
    return (
      <div className="nav-link-wrapper">
        <NavLink to="/blog" activeClassName="nav-link-active">
          Blog
        </NavLink>
      </div>
    );
  };
```

to this, because the route and the text are passed in already (see above).

```
 const dynamicLink = (route, linkText) => {
    return (
      <div className="nav-link-wrapper">
        <NavLink to={route} activeClassName="nav-link-active">
          {linkText}
        </NavLink>
      </div>
    );
  };
  ```

  ## Portfolio Manager Grid Layout Implementation

  Now we're going to create the grid for the manager. In the **portfolio-manager.js** file, we'll create the structure for the grid, as well as its own scss page (called manager.scss):

  ```
  import React, { Component } from 'react';

export default class PortfolioManager extends Component {
    constructor() {
        super();

}
    render() {
        return (
        <div className='portfolio-manager-wrapper'>
            <div className='left-column'>
                <h1>Portfolio form</h1>
            </div>

            <div className='right-column'>
                <h1>Portfolio form</h1>
            </div>
        </div>
        );
    };
}
```

```
.portfolio-manager-wrapper {
    display: grid;
    grid-template-columns: 3fr 1fr;

    .left-column {
        background-color: $offwhite;
    }

    .right-column {
        background-color: $charcoal;
    }
}
```

