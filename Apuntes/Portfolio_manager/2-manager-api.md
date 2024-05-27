## Populating the Portfolio Manager State with API Call Data

In this guide, we're gonna walk through how we can pull in our portfolio items and then we will eventually add them into the sidebar.

In the **portfolio-manager.js** file, we're going to add a state that will start empty. We'll import axios as well.

```
export default class PortfolioManager extends Component {
    constructor() {
        super();

        this.state= {
            portfolioItems: []
        }
}
```

```
import React, { Component } from 'react';
import axios from "axios"

export default class PortfolioManager extends Component {
    constructor() {
        super();

        this.state= {
            portfolioItems: []
        }
}

getPortfolioItems() {
    axios.get("https://garazils.devcamp.space/portfolio/portfolio_items", { 
    withCredentials: true
  }).then(response => {
    console.log("got items", response)
  }).catch(error => {
    console.log("error found", error)
  })
}

componentDidMount() {
this.getPortfolioItems();  
}
```

> Console logs are for testing purposes only. Remember to use the ComponentDidMount to call the function every time the component gets mounted.

Now we're going to update the state with the data we saw in the console:

```
getPortfolioItems() {
    axios.get("https://garazils.devcamp.space/portfolio/portfolio_items", { 
    withCredentials: true
  }).then(response => {
    this.setState({
        portfolioItems: [...response.data.portfolio_items]
    })
  }).catch(error => {
    console.log("error found", error)
  })
}
```

> We're using a spread operator because we're getting an array with items, if we weren't using that we would get everything in a single item.

## Building the Portfolio Sidebar List Component

Now that we have all of our data residing inside of our portfolio managers state, what we need to do is create a component. Inside the src > components > portfolio-container directory we're going to create a new file: ```portfolio-sidebar-list```. We'll write whatever we want just to test it and also will import it from the portfolio manager.

```
import React from 'react';

const PortfolioSidebarList = (props) => {
    return <div>List...</div>
}

export default PortfolioSidebarList;
```

```
// portfolio-manager.js
import React, { Component } from 'react';
import axios from "axios"

import PortfolioSidebarList from '../portfolio-components/portfolio-sidebar-list';

(...)

<div className='right-column'>
    <PortfolioSidebarList/>
</div>
```

Now from that component in the portfolio manager we'll pass a prop called data which will carry the PortfolioItems state, which will in turn be populated when we call the ```getPortfolioIntems()``` method. We're passing all of this to a prop called data which will be accessible from the PortfolioSidebarList.

```
<div className='right-column'>
    <PortfolioSidebarList data={this.state.portfolioItems}/>
</div>
```

Now we'll work from the **PortfolioSidebarList.js** file to add a function that maps over a collection of data.

```
const PortfolioSidebarList = (props) => {
    const PortfolioList = props.data.map(PortfolioItem => {
       
    })
}
```

> Whenever we want to iterate over a collection of data and to build up a full component of data then we use the map method.

>>  We use props.data because data is the name of the prop that we're getting passed. If we called this portfolioItems or anything like that then we would call it that. But we got a prop called data, and then from there, we're just going to map over that function. And each time we map over, remember the way map works, each time we get access to one element, and then you have to name that element, so I think it makes sense to call this portfolio item.

From there, we'll add a return statement that will receive the key and its properties: (visible from the console).

To render this, we'll call the PortfolioList variable, which carries all of the things we've discussed:

```
import React from 'react';

const PortfolioSidebarList = (props) => {
    const PortfolioList = props.data.map(PortfolioItem => {
        return (
            <div>
                <h1>{PortfolioItem.name}</h1>
                <h2>{PortfolioItem.id}</h2>
            </div>
        )
    })
    
    return <div>{PortfolioList}</div>
}
```

Now all of the items will be rendered inside the right column. we only need to add the thumbnail:

```
const PortfolioSidebarList = (props) => {
    const PortfolioList = props.data.map(PortfolioItem => {
        return (
            <div>
                <div>
                    <img src={PortfolioItem.thumb_image_url} />
                </div>
                <h1>{PortfolioItem.name}</h1>
                <h2>{PortfolioItem.id}</h2>
            </div>
        )
    })
    
    return <div>{PortfolioList}</div>
}
```

Now we'll add styles and wrap everything we have. Inside the **portfolio-sidebar-list.js** file we'll wrap the img and the h tags inside a div. The wrapper class is inside the calling of the constant:

```
const PortfolioSidebarList = (props) => {
    const PortfolioList = props.data.map(PortfolioItem => {
        return (
            <div className='portfolio-item-thumb'>
                <div className='portfolio-thumb-img'></div>
                    <div>
                        <img src={PortfolioItem.thumb_image_url} />
                    </div>
                    <h1 className='title'>{PortfolioItem.name}</h1>
                    <h2>{PortfolioItem.id}</h2>
            </div>
        )
    })
    
    return <div className='portfolio-sidebar-list-wrapper'>{PortfolioList}</div>
    ```