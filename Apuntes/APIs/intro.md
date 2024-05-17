## Intro

Until now we've been hardcoding data. Now we'll get it from APIs (Application Programming Interface). We do this by passing URLs to the server and it brings data back (it's more complicated).

![devcamp space apis](https://s3-us-west-2.amazonaws.com/images-devcamp/Dissecting+React+JS/API+Communication/Overview+of+DevCamp+Space+%23+2365/image14.png)

After adding a record in the DevCamp space, if we click on All items' link we'll get this raw data:

![raw data](https://s3-us-west-2.amazonaws.com/images-devcamp/Dissecting+React+JS/API+Communication/Overview+of+DevCamp+Space+%23+2365/image16.png)

Now we have to pull this into our app. To do that we're going to pull a library that allows to access different API endpoints called Axios, and we're going to import in the portfolio app.js file.

```
import React, { Component } from 'react';
import moment from "moment";
import axios from "axios";
import {BrowserRouter as  Router, Switch, Route } from "react-router-dom"
```

## Initial GET request

Right below the class creation line, with the render function below and before the constructor (remember to add it again) we're gonna create a function and inside it paste this code (it's on the npm axios documentation):

```
getPortfolioItems() {
    // Make a request for a user with a given ID
axios.get('/user?ID=12345')
.then(function (response) {
  // handle success
  console.log(response);
})
.catch(function (error) {
  // handle error
  console.log(error);
})
.finally(function () {
  // always executed
});
  }
```

Back in devcamp space, we'll grab the all items link from the portfolio. This is an open API replica, but some real APIs require authentication (we don't want anyone to create API requests, after all). In the axios.get function we'll paste the link we copied:

```
getPortfolioItems() {
    // Make a request for a user with a given ID
axios.get('https://subdomain.devcamp.space/portfolio/portfolio_items')
.then(function (response) {
  // handle success
  console.log(response);
})
.catch(function (error) {
  // handle error
  console.log(error);
})
.finally(function () {
  // always executed
});
  }
```

Now we'll change the ```.then(function (response) {...}``` portion, as well as the error one, with an arrow function. This is specific to react. 

```
axios.get('https://subdomain.devcamp.space/portfolio/portfolio_items')
.then(response => {
  // handle success
  console.log(response);
})
  
.catch(error => {
   // handle error
   console.log(error);
  })
 
.finally(function () {
  // always executed
});
```

> Later on we'll render this changes on the page. The call will happen into the render function for now, for testing purposes.

We now have this (everytime the render function is called the getportfolioitems() contacts the API and logs whatever response is).

```
export default class App extends Component {
  constructor() {
    super();

  this.getPortfolioItems = this.getPortfolioItems.bind(this);
}
  getPortfolioItems() {
    // Make a request for a user with a given ID
axios.get('https://subdomain.devcamp.space/portfolio/portfolio_items')
.then(response => {
  // handle success
  console.log(response);
})
  
.catch(error => {
   // handle error
   console.log(error);
  })
 
.finally(function () {
  // always executed
});
  }
  render() {
    this.getPortfolioItems();
        return (...)}
}
```
Now we're getting live data on the app.

> Before creating a component in its definitive place first test it out in the main app. You'll move it later. If a bug is in the component you wouldn't know if it's on a library or in the component.

### Organizing code

We're going to move the API requests into the portfolio container. Remember to import axios as well. We now have this in the portfolio container:

```
import React, {Component} from "react";
import axios from "axios";

import PortfolioItem from "./portfolio-item"

export default class PortfolioContainer extends Component {
    constructor() {
        super(); 
        this.state = {
            pageTitle: "Welcome to my portfolio",
             data: [
                {title: "First", category: "eCommerce", slug: "first"},
                 {title: "Second", category: "eCommerce", slug: "second"},
                 {title: "third", category: "Personal", slug: "third"}, 
                 {title: "fourth", category: "Hobby", slug: "fourth"}
                ],
            isLoading: false
            };

            this.handleFilter = this.handleFilter.bind(this);
            this.getPortfolioItems = this.getPortfolioItems.bind(this);
    }

    handleFilter(filter) {
        this.setState({
            data: this.state.data.filter(item => {
                return item.category === filter;
            })
        })
    }

    getPortfolioItems() {
        // Make a request for a user with a given ID
    axios.get('https://subdomain.devcamp.space/portfolio/portfolio_items')
    .then(response => {
      // handle success
      console.log("response data", response);
    })
      
    .catch(error => {
       // handle error
       console.log(error);
      })
     
    .finally(function () {
      // always executed
    });
      }

    portfolioItems() {
        return this.state.data.map(item => {
          return (
            <PortfolioItem title={item.title} url={"google.com"} slug={item.slug} />
          );
        });
      }

    render() {
        if(this.state.isLoading) {
            return <div>Loading...</div>
        }
        this.getPortfolioItems()
        return (
            <div>
                <h2>{this.state.pageTitle}</h2>

                <button onClick={() => this.handleFilter("eCommerce")}>eCommerce</button>
                <button onClick={ () => this.handleFilter("Personal")}>Personal</button>
                <button onClick={() => this.handleFilter("Hobby")}>Hobby</button>

                {this.portfolioItems()}
            </div>
        )
    }}
```

