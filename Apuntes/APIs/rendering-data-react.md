## Rendering API data in react

We're going to render data on the screen, so the hard-coded data entry points can now be deleted. The data's initial state will start empty. When the API call comes, the state will be updated.

```
export default class PortfolioContainer extends Component {
    constructor() {
        super(); 
        this.state = {
            pageTitle: "Welcome to my portfolio",
             data: [],
            isLoading: false
            };
    }}
```

We're also going to remove the getPortfolioItems() bind because we're not going to call this from the render function: instead, we'll use of react's lifecycle hooks.

```
portfolioItems() {
        return this.state.data.map(item => { // looping through data 
          return (
            <PortfolioItem title={item.title} url={"google.com"} slug={item.slug} />
          );
        });
      }

      componentDidMount() {
        this.getPortfolioItems()
      }

    render() {...}
```

After the component is mounted the API call will take effect. To update the data we'll update the getPortfolioItems() state:
 
 ```
 getPortfolioItems() {
     // Make a request for a user with a given ID
 axios.get('https://subdomain.devcamp.space/portfolio/portfolio_items')
 .then(response => {
   // handle success
   console.log("response data", response);
   this.setState({
     data: response.data.portfolio_items // data gets the API items
   })
 })}
 ```

> To know which items call, go to the console in Chrome and search for the data object (call it via a console.log first), it will have the keys. The *response* element can be renamed.

```
data
: 
portfolio_items
: 
Array(3)
0
: 
banner_image_url
: 
"https://devcamp-space.s3.amazonaws.com/W9PvPD7HC7eb1CsRPzkyrZsk?response-content-disposition=inline%3B%20filename%3D%22open-devos.jpg%22%3B%20filename%2A%3DUTF-8%27%27open-devos.jpg&response-content-type=image%2Fjpeg&X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAJEHZJNHM5JFESRRQ%2F20240517%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20240517T100356Z&X-Amz-Expires=300&X-Amz-SignedHeaders=host&X-Amz-Signature=7c80cc9eaf69ecc346f81352e118392e19117cb6e0bfe61c108eac9a0b58a4d8"
category
: 
"Personal"
column_names_merged_with_images
: 
(9) ['id', 'name', 'description', 'url', 'category', 'position', 'thumb_image', 'banner_image', 'logo']
description
: 
"Lorem ipsum dolor sit amet"
id
: 
42440
logo_url
: 
"https://devcamp-space.s3.amazonaws.com/C2qNXwzEBuWo3qJEuuE9vZNc?response-content-disposition=inline%3B%20filename%3D%22devcamp.png%22%3B%20filename%2A%3DUTF-8%27%27devcamp.png&response-content-type=image%2Fpng&X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAJEHZJNHM5JFESRRQ%2F20240517%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20240517T100356Z&X-Amz-Expires=300&X-Amz-SignedHeaders=host&X-Amz-Signature=480d7bcc0fbbc1885eded132d2acf256c21b8774553489f367e9cb4da2168bdd"
name
: 
"Project 2"
position
: 
2
thumb_image_url
: 
"{https://devcamp-space.s3.amazonaws.com/oDxVd9avsfM2VNktyd1ZZxRP?response-content-disposition=inline%3B%20filename%3D%22open-devos.jpg%22%3B%20filename%2A%3DUTF-8%27%27open-devos.jpg&response-content-type=image%2Fjpeg&X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAJEHZJNHM5JFESRRQ%2F20240517%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20240517T100356Z&X-Amz-Expires=300&X-Amz-SignedHeaders=host&X-Amz-Signature=ba7eb5bd0c7fc3acb95d6fd812f3da347484e4ab11fd9a0f4eade3c88be4b023}"
url
: 
"https://loremipsum.com"
```

Following the same process as above we can now update the portfolioItem component to render the API data on the screen:

```
<PortfolioItem title={item.name} url={item.url} slug={item.id}
```

## Key prop in react

The key prop is what react uses to keep track of elements. We can treat it like any other prop. Keys must be unique to each element, and IDs, in fact, are that.

```
<PortfolioItem key={item.id} title={item.name} url={item.url} slug={item.id}
```

Now the warning in the console will be gone.

## Console log data from an API

> Console log the data. In the console you'll see its elements and all of its metadata.

```
portfolioItems() {
        return this.state.data.map(item => { // looping through data items, that now have API data.
          console.log("data", item)
        })}

```

## Debugger

The debugger, if typed in the app, will stop the execution of the program, but it still allows to run scripts. This allows to access the data insde the variables step by step, line by line. States aren't accessible though.

We're now going to access to the data and to render in on the screen.

To do so, write in the console ```Object.keys(object)```. This will get an object's keys. Later on we'll use this to  translate this data and pass it directly into our portfolio item and we're also going to use some best practices to wrap all of this data up inside of our props object.

```
item
: 
{id: 42442 ...}
```

> The object is accessible through Chrome's dev tool fonts, with the debugger mode active.

```
0: "id"
1: "name"
2: "description"
3: "url"
4: "category"
5: "position"
6: "thumb_image_url"
7: "banner_image_url"
8: "logo_url"
9: "column_names_merged_with_images"
length: 10 
```

We also have the ability to pass an object as the key inside props.

So this (with the values that came from the API)

```
portfolioItems() {
  return this.state.data.map(item => { // looping through data items, that now have API data.
    console.log("data", item)
    return (
      <PortfolioItem key={item.id} title={item.name} url={item.url} slug={item.id} />
```

will become this:

```
<PortfolioItem key={item.id} item={item} />
```

In the portfolio-item file we'll perform de-structuring:

```
const {id, description, thumb_image_url, logo} = props.item
```

So we won't need this ```<div><h3>{props.title}</h3> / <h4>{props.url}</h4>``` and we can also transform this 

```
  <Link to={`/portfolio/${props.slug}`}>Link</Link>
  </div>
```

into this: 

```
<Link to={`/portfolio/${id}`}>Link</Link>
```

This is possible because of the de-structuring, because we grabbed the id directly from *props.item*.


## Rendering images from an API

Rendering images from an API is very similar to vanilla HTML. In the portfolio-item, we'll type this (remenber the de-structuring):

```
const {id, description, thumb_image_url, logo_url} = props.item
    return(
        <div>
            <img src={thumb_image_url}/>
            <img src={logo_url}/>
```