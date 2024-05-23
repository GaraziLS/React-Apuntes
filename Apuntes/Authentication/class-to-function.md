We've seen how to convert from a functional component to a class component. 

```
> In the *portfolio-item.js* file, we're going to convert the functional component into a class component to manipulate state. So from this:

export default function(props) {
  const { id, description, thumb_image_url, logo_url } = props.item;
  return (
    <div className="portfolio-item-wrapper">
      <div
        className="portfolio-img-background"
        style={{
            backgroundImage: "url(" + thumb_image_url + ")"
        }}
      />
  )}

to this:

export default class PortfolioItem extends Component {
  constructor(props) {
    super(props);

    this.state = {
      portfolioItemClass: ""
    };
  }}
```


Now we'll do the opposite: from a class component we'll go to a functional component.

So in the **NavigationContainer.js** file, we'll remove the component part in the react import:

```
import React from 'react';
```

Intead of this:

```
export default class NavigationContainer extends Component {
    constructor() {
        super();
    }
```

We'll say this:

```
import React from "react";
import { NavLink } from "react-router-dom";

const NavigationComponent = props => {
  return (
    <div className="nav-wrapper">
      <div className="left-side">
        <div className="nav-link-wrapper">
          <NavLink exact to="/" activeClassName="nav-link-active">
            Home
          </NavLink>
        </div>

        <div className="nav-link-wrapper">
          <NavLink to="/about-me" activeClassName="nav-link-active">
            About
          </NavLink>
        </div>

        <div className="nav-link-wrapper">
          <NavLink to="/contact" activeClassName="nav-link-active">
            Contact
          </NavLink>
        </div>

        <div className="nav-link-wrapper">
          <NavLink to="/blog" activeClassName="nav-link-active">
            Blog
          </NavLink>
        </div>
      </div>

      <div className="right-side">JORDAN HUDGENS</div>
    </div>
  );
};

export default NavigationComponent;
```

And will still be working.