## Creating the Portfolio Form and Calling it from the Parent Component

We need to create a portfolio form component and call it from the portfolio manager. Inside of the **portfolio container** folder we're going to create a new file, **portfolio-form**, and import it into the portfolio-manager.js.

```
import React, { Component } from 'react';

export default class PortfolioForm extends Component {
    constructor() {
        super();

}
    render() {
        return (
        <div>
            Test
        </div>
        );
    };
}
```

Now, in the **portfolio-manager.js** file, we know that we'll have to update the portfolio items state because we'll create new items when we hit the submit button. To do that we'll pass functions as props.

First we'll create and bind a couple of handlers:

```
this.handleSuccessfulFormSubmission = this.handleSuccessfulFormSubmission.bind(this)
        this.handleFormSubmissionError = this.handleFormSubmissionError.bind(this)

handleSuccessfulFormSubmission(portfolioItem) {

}

handleFormSubmissionError(error) {

}
```

And inside the call of the component we'll pass the two handlers:

```
render() {
    return (
    <div className='portfolio-manager-wrapper'>
        <div className='left-column'>
            <PortfolioForm
            handleSuccessfulFormSubmission={this.handleSuccessfulFormSubmission}
            handleFormSubmissionError={this.handleFormSubmissionError}
            />
        )}
```

 ## Building the initial portfolio form elements

 > Inside the **portfolio-form.js** file, we first need to capture each one of the data elements inside of state, so we're gonna have to create state, and then we are gonna list out each one of the elements that we're gonna be using. Then from there, we need to add the form. Inside the form, we're gonna have the data input type, the name, and the value. Then from there, remember, if we only have those two things, we are not actually gonna be able to type anything in, because the value is gonna be the same as state, so we need a way of connecting the two, we need a way of handling and listening for change events in the form so that we can update state. So in this guide, let's do two out of the three items. In this guide, we're going to list out and define our state, so that we have a starting base state, and then we're gonna go and create the form elements. And then in the next guide, we'll create a handler that allows us to update that state.

Now, inside the **portfolio-form.js** file we'll be adding props to the constructor because we'll be passing props. We'll also create the initial state:

```
import React, { Component } from 'react';

export default class PortfolioForm extends Component {
    constructor(props) {
        super(props);

        this.state = {
            name: "",
            description: "",
            category: "",
            position: "",
            url: "",
            thumb_image: "",
            banner_image: "",
            logo: ""
          };
}
```

Now we're able to create the form. Remember that the input types (the values) must be matching the state keys.

```
<form>
          <div>
            <input
              type="text"
              name="name"
              placeholder="Portfolio Item Name"
              value={this.state.name}
              onChange={this.handleChange}
            />

            <input
              type="text"
              name="url"
              placeholder="URL"
              value={this.state.url}
              onChange={this.handleChange}
            />
          </div>

          <div>
            <input
              type="text"
              name="position"
              placeholder="Position"
              value={this.state.position}
              onChange={this.handleChange}
            />

            <input
              type="text"
              name="category"
              placeholder="Category"
              value={this.state.category}
              onChange={this.handleChange}
            />
          </div>

          <div>
            <input
              type="text"
              name="description"
              placeholder="Description"
              value={this.state.description}
              onChange={this.handleChange}
            />
          </div>

          <div>
            <button type="submit">Save</button>
          </div>
        </form>
      </div>
```

## Implementing Portfolio Form Handlers

If we type inside the form fields right now, nothing is going to happen. That's because the connecting piece (an event handler) is missing. **Inside the portfolio-form.js** file, we're going to type this:

```
this.state = {
      name: "",
      description: "",
      category: "",
      position: "",
      url: "",
      thumb_image: "",
      banner_image: "",
      logo: ""
    };

    this.handleChange = this.handleChange.bind(this);
    this.handleSubmit = this.handleSubmit.bind(this);
  }

  handleChange(event) {
    this.setState({
      [event.target.name]: event.target.value
    });
  }

  handleSubmit(event) {
    console.log("event", event);
    event.preventDefault();
  }
```

Now the text fields will work, and when hitting the submit button the page won't reload. Now we have to add the Submit event handler to the form: ```<form onSubmit={this.handleSubmit}>```. (Notice that the Change handler is already included in the OnChange portion of the form).

## Intro to FormData objects

FormData Objects allow us to work with non-text objects, such as images. In the **portfolio-form.js** file we're going to create a function called BuildForm(), just below the bindings and above the handlers. Inside this function we're going to create the FormData object. Inside of there we'll add data points using the append method:

```
BuildForm() {
    let formData = new FormData();

    formData.append()
}
```

The append method takes two arguments: The first is going to be a string and it's going to be the string and structure that our API expects. That structure is going to be portfolio, underscore item, and then name, and name is gonna be in brackets, and then with a comma, 'cause we're making a second argument ``this.state.name``.

```
BuildForm() {
  let formData = new FormData();

  formData.append("portfolio_item[name]", this.state.name);
}
```

We're gonna repeat this for the other form elements as well.

Now we'll return the full object, and need to call it as well:

```
BuildForm() {
    let formData = new FormData();
  
    formData.append("portfolio_item[name]", this.state.name);
    formData.append("portfolio_item[desccription]", this.state.desccription);
    formData.append("portfolio_item[url]", this.state.url);
    formData.append("portfolio_item[category]", this.state.category);
  
    return formData;
}
```

That calling will be placed inside the handleSubmit() event.

```
handleSubmit(event) {
    this.BuildForm();
    event.preventDefault();
  }
```
  Now everything is wrapped and can be passed to the API.

 > Remember to give unique keys to items inside collections, or errors will pop up.

 ## Creating Portfolio Items from React Form

 We can connect to the API and create records from the form that will be submitted to the API. In **Devcamp space**, inside the portfolio items, we have different endpoints. We'll get the *Create an Item* one.

 Inside the **portfolio-form.js** file, inside the handleSubmit, we'll call axios and pass in a post request that includes the url, the BuildForm function, and the WithCredentials object to let the server know our identity.

 ```
 handleSubmit(event) {
    axios.post("https://garazils.devcamp.space/portfolio/portfolio_items", this.BuildForm, {withCredentials: true})
    this.BuildForm();
    event.preventDefault();
  }
  ```
Now we're going to build the promise, and now we have the ability to create new records from the app:

```
 handleSubmit(event) {
    axios
      .post(
        "https://garazils.devcamp.space/portfolio/portfolio_items",
        this.buildForm(),
        { withCredentials: true }
      )
      .then(response => {
        console.log("response", response);
      })
      .catch(error => {
        console.log("portfolio form handleSubmit error", error);
      });

    event.preventDefault();
  }
```

## Select Dropdown in React

The description of the form should be a text area and the category should be a dropdown. In the category input we'll change that input to a select tag. The type and the placeholder can be removed, so we get this:

```
<select
  name="category"
  value={this.state.category}
  onChange={this.handleChange}
    />
  </div>
```

Now inside it we need to include the options.

```
<select
    name="category"
    value={this.state.category}
    onChange={this.handleChange}

    <option value="Bussiness">Bussiness</option>
    <option value="Comissions">Comissions</option>
    <option value="Personal">Personal</opt
    </select>
</div>
```

## Text area

All we have to do is come to an imput and change the tag with ```textarea``.

```
<textarea
     type="text"
     name="description"
     placeholder="Description"
     value={this.state.description}
     onChange={this.handleChange}
   />
 </div>
```

## Implementing a Base State Value for a React Select Tag

Our application has a small bug in it. That bug is that even though our form begins with a default value for the category, when we hit save, no category is getting pushed up.

The select tag only works when it's been changed (because it has the handleChange method on it), and because the default state is empty. To fix this, we can simply update the string with a default value:

```
this.state = {
      name: "",
      description: "",
      category: "Bussiness",
      position: "",
      url: "",
      thumb_image: "",
      banner_image: "",
      logo: ""
    };
```

This way, the constructor is going to run when the class is initiated.

# Connect the portfolio form with the portfolio manager

Now we'll create a feature that connects the form with the sidebar so when we create a new record from the form it applears on the sidebar.

In the **portfolio-manager.js** file we're going to copy the ``handleSuccessfulFormSubmission`` portion and paste it in the handle submit's then promise (inside the **portfolio-form**):

```
handleSubmit(event) {
    axios
      .post(
        "https://garazils.devcamp.space/portfolio/portfolio_items",
        this.buildForm(),
        { withCredentials: true }
      )
      .then(response => {
        this.props.handleSuccessfulFormSubmission
        console.log("response", response);
      })
}
```
_______________ **REVIEW PORTION**_______________________

> We have a default state of portfolioItems which is an array and then when the component mounts we're calling getPortfolioItems and we are populating that array.

```
this.state= {
  portfolioItems: []
(...)

getPortfolioItems() {
    axios.get("https://garazils.devcamp.space/portfolio/portfolio_items", { 
    withCredentials: true
  }).then(response => {
    this.setState({
        portfolioItems: [...response.data.portfolio_items]
    }))}

    (...)

    componentDidMount() {
this.getPortfolioItems();  
}}
```

_____________**END OF REVIEW PORTION**___________________

Now inside the ``handleSuccessfulFormSubmission`` (in the **portfolio-manager.js** file) function we'll update state. 

```
handleSuccessfulFormSubmission(portfolioItem) {
    this.setState({
        portfolioItems: [portfolioItem]
    })
}
```

So we're putting the portfolioItem (which is created inside the **portfolio-sidebar.js**)

```
const PortfolioSidebarList = (props) => {
    const PortfolioList = props.data.map(PortfolioItem => {
        return (
```

 inside an array. From there we're going to call the concat method to concatenate the other records by calling the state directly.

```
handleSuccessfulFormSubmission(portfolioItem) {
    this.setState({
        portfolioItems: [portfolioItem].concat(this.state.portfolioItems)
    })
}
```

> The connection works because we passed props to the portfolio-sidebar-list.

Now the new items appear in the sidebar, on top of the others but if we refresh, the API puts everything all the way down. To fix this (specific to the API we're using), in the **portfolio-manager.js** file, in the url, we'll type this: ```?order_by=created_at&direction=desc```. Now everything is sorted descendantly. Most APIs have optional parameters.


