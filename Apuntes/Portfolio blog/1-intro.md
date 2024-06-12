## Intro

We already have a blog component (inside the src > components > pages folder), so the initial structure is there. We'll keep working on that. We'll change the blog component/page into a class component. So we'll go from this:

```
import React from 'react';
import {Link} from 'react-router-dom';

export default function() {
    return(
    <div>
        <h2>Blog</h2>
        <div>
            <Link to="/about-me">Read more about me</Link>
        </div>
    </div>
);
}
```

To this:

```
import React, { Component } from 'react';
import { Link } from 'react-router-dom';

class Blog extends Component {
    constructor() {
        super();
    }
    render() {
        return (
            <div>
                <h2>Blog</h2>
                <div>
                    <Link to="/about-me">Read more about me</Link>
                </div>
            </div>
        );
    }
}

export default Blog;
```

## Calling the Blog API and Storing the Data in State

Now that we have our class component in place, we're ready to actually call the blog API and then return our data, so we'll first import axios inside the blog component.

Now that we know that we can contact an API, we are also going to have to store our blog records inside of our local state, so we need to instantiate the state and give a base set of values, a base state.

So we'll create a state:

```
class Blog extends Component {
    constructor() {
        super();

        this.state = {
            blogItems: []
        }
    }
```

And now a function that's going to retrieve those records, which we'll bind too, so we should get this:

```
class Blog extends Component {
  constructor() {
    super();

    this.state = {
      blogItems: []
    };

    this.getBlogItems = this.getBlogItems.bind(this);
  }


getBlogItems() {
  axios
    .get("https://jordan.devcamp.space/portfolio/portfolio_blogs", {
      withCredentials: true
    })
    .then(response => {
      this.setState({
        console.log("response", response);
      });
    })
    .catch(error => {
      console.log("getBlogItems error", error);
    });
}
```
We'll now create a lifehook with the function.

```
componentWillMount() {
    this.getBlogItems();
  }
```

Once we get our response back, we can see the data attribute and inside of it more tags to work with items and call ``.response.data.portfolio_blogs`` to populate the state. To do that, when the API response comes back, we'll set state:

```
 getBlogItems() {
    axios
      .get("https://jordan.devcamp.space/portfolio/portfolio_blogs", {
        withCredentials: true
      })
      .then(response => {
        this.setState({
          blogItems: response.data.portfolio_blogs
        });
      })
```

and call those data tags that we saw in the console. Now we should be able to see the record in the console. Now, the API only shows 10 items, that's because we'll implement infinite scrolling later.

## Rendering Blog Records to the Screen

Now that we have our blog items in our local component state, we can work with 'em and we can render them on the screen.

We're still in the **blog.js** page. We'll now create a variable that stores all the blog records. The variable will loop over each item with a map function, and we'll do that over the setState blogItems variable. That's going to be stored inside an array:

```
render() {
    const blogRecords = this.state.blogItems.map(blogItem => {
        return <h1>{blogItem.title}</h1>
    })
```

Now inside the render's return we can call blogRecords:

```
 render() {
    const blogRecords = this.state.blogItems.map(blogItem => {
        return <h1>{blogItem.title}</h1>
    })
    return (
      <div>
        {blogRecords}
      </div>
```

This will render the titles on the screen. We're not calling the this word because we are inside the render method.

## Creating a Dedicated Blog Item Component

So, now that we're rendering our titles on the screen, in this guide we're gonna create a dedicated item, a dedicated blog item component.

We'll create a new folder inside the src > components called blogs. Inside this we'll have all the blog related components, and will start with a **blog-item.js** file.

We'll create a new function there in a slightly different way. This:

```
const BlogItem = props => {

}

export default BlogItem;
```

is another way of writing this:

```
export default funciton BlogItems() {

}
```

The blog component is going to send us the entire BlogItem object and so, that is what we should expect to receive. Now, inside of that BlogItem object are gonna be a number of attributes and we can look at DevCamp Space to see what we're going to get.

So, if I go to Blog, I can see we're gonna get an id, a title, content, a blog_status and then also featured_image and if you remember from our portfolio, we need to also add a _url because that's what the API returns.

Thus, we know that we'll recieve an onject with those values. We now need to de-structure that, so we'll get this (the blogItem portion comes from the map loop in the **blog.js** file):

```
 render() {
    const blogRecords = this.state.blogItems.map(blogItem => {
        return <h1>{blogItem.title}</h1>
    })
```

So we get this with the de-structuring:

```
import React from "react";

const BlogItem = props => {
    const {
            id,
            blog_status,
            content,
            title,
            featured_image_url
          } = props.blogItem
        }
        
        export default BlogItem;
```

Now we can return some JSX so we would get this:

```
import React from "react";

const BlogItem = props => {
  const {
    id,
    blog_status,
    content,
    title,
    featured_image_url
  } = props.blogItem;

  return (
    <div>
      <h1>{title}</h1>
      <div>{content}</div>
    </div>
  );
};

export default BlogItem;
```

Now we need to import this into the **blog.js** file, and then we'll call it inside the render function (remember to write a unique key, and to pass the prop itself):

```
import BlogItem from "../blogs/blog-item"

()...)

render() {
    const blogRecords = this.state.blogItems.map(blogItem => {
        return <BlogItem key={blogItem.id} blogItem={blogItem} />
    })
    return (
      <div>
        {blogRecords}
      </div>
    );
  }
}
```

Now the blogRecords variable contains the blogItem inside, whch in turn contains the titles and the content.

## Building the Initial Blog Detail Component

In this guide we're gonna build out the blog detail component and we're gonna connect it both with the routing engine and also with our blog index page. We want to be able to click on one of these titles and be taken to that blog detail component.

In the **app.js** file we're going to import a BlogDetail page (we have to create it, as well as the route).

Now we only have to connect the blog and the blog detail page to access the latter from the blog. We'll open the **blog-item.js** file and we'll import the link tag. Then, we'll put a link to the blog detail while wrapping the title to get a direct link to each title.

```
return (
  <div>
    <Link to={'/b/${id}'}>
      <h1>{title}</h1>
    </Link>
    <div>{content}</div>
  </div>
);
```

> To see this you must be logged in.

## Calling a Single Blog Item from the API in React

We're gonna start building out our blog detail component. We'll first create a constructor inside the **blog-detail.js** page. Inside the state, we'll going to write the current id and pass this to grab the current id later:

```
export default class BlogDetail extends Component {
    constructor(props) {
        super(props);

        this.state ={
            currentID: this.props.match.params.slug,
            blogItem: {}
        }
```

Now that we have that, we know we're going to have to populate our current blog, so what I'm gonna do is give us a starting state for our blogItem, and the starting state's just gonna be an empty object, so whenever BlogDetail's initialized, it's gonna start off as an empty object, and then what we're gonna do is we're gonna replace this with the response from the API.

So we'll need to call axios again, and now we'll create the method.

```
getBlogItem() {
    axios.get(`https://garazils.devcamp.space/portfolio/portfolio_blogs/${this.state
      .currentID}`)
```

> We don't need to pass the With Credentials item because to see the blog no login is required.

Now we have to make sure that that's triggered whenever the component has been loaded, so I'm gonna add a lifecycle method here called component, and this time, I'm just gonna say, componentDidMount and now I can call this.getBlogItem and call it as a regular function.

```
componentDidMount() {
    this.getBlogItem();
  }
```

Now that we have the data coming in from the server, in the next guide, we're gonna update our component state and then we're going to start rendering the content onto the screen.

## Rendering the Blog Details to the Screen

So now that we know that we can properly call the API, and we can get a single blog item back, now let's render it on the screen. If you look at the response object, we can grab the response, and then traversing the tree, we can move down, we can grab the data, and then inside of the data, we can grab the portfolio_blog. That is what we're gonna be able to use to populate our local item state.

Still in the **blog-detail.js** item, we'll set state to the blogItem inside that axios promise of above:

```
getBlogItem() {
    axios.get(`https://garazils.devcamp.space/portfolio/portfolio_blogs/${this.state
      .currentID}`).then(response => {
        console.log("response", response);
        this.setState({
            blogItem: response.data.portfolio_blog
        })
```

let's see if this works and see if it populates our state using the react devtools, it should show the blogItem tag with the data from the API, and now we can render it on the screen via the render method.

```
getBlogItem() {
    axios.get(`https://garazils.devcamp.space/portfolio/portfolio_blogs/${this.state
      .currentID}`).then(response => {
        console.log("response", response);
        this.setState({
            blogItem: response.data.portfolio_blog
        })
      }).catch(error => {
        console.log("getBlogItem", error)
      })
}

 componentDidMount() {
    this.getBlogItem();
  }


    render() {
        const {
            title,
            content,
            featured_image_url,
            blog_status
          } = this.state.blogItem;
        console.log(this.state.currentID)
        return (
            <div>
                <h1>{title}</h1>
                <img src={featured_image_url} />
                <div>{content}</div>
            </div>
        );
    };
}
```

Now we can add styles.

