## Building an Edit Click Handler in React

Now that we have the delete functionality completed, we've completed three out of the four CRUD features. Remember, CRUD stands for create, read, update, and delete. Only the Update part remains.

To start with this, we'll go to the **portfolio-manager.js** file to add another item to the state, that will start as an empty object:

```
   this.state = {
      portfolioItems: [],
      portfolioToEdit: {}
    };
```

Now that we have that in state, the next thing that we need to do is create a function that we are going to pass to the sidebar to listen for that click event. We'll bind it as usual. This state has now the portfolioItem-s passed in.

```
  handleEditClick(portfolioItem) {
    this.setState({
      portfolioToEdit: portfolioItem
    })
  }
```

Now we'll pass this to the portfolio sidebar component that's called in this file, so it now has access to it via props.

```
<PortfolioSidebarList
       handleDeleteClick={this.handleDeleteClick}
       data={this.state.portfolioItems}
       handleEditClick= {this.handleEditClick}
/>
```

Despite of this, we have to create a link in the **portfolio-sidebar-list.js** component.

```
<div className='icons'>
           <a className="icon" onClick={() => props.handleEditClick(PortfolioItem)}>Edit</a>
           <a className="icon" onClick={() => props.handleDeleteClick(PortfolioItem)}><FontAwesomeIcon icon= "trash"/></a>
       </div>
```

## Populating the Portfolio Edit Form in React

Now that we're effectively updating state every time that a user clicks on the edit icon here, the next step is to connect our portfolio form with our portfolio manager for the edit functionality.

In the **portfolio-manager.js** file we'll create a new function to clear the portfolio to edit the state and to return it to the initial state:

```
 clearPortfolioToEdit() {
    this.setState({
      portfolioToEdit: {}
    });
  }
```

We'll call this in the PortfolioForm calling, as well as the data (actually a call to the state).

```

  render() {
    return (
      <div className="portfolio-manager-wrapper">
        <div className="left-column">
          <PortfolioForm
            handleSuccessfulFormSubmission={this.handleSuccessfulFormSubmission}
            handleFormSubmissionError={this.handleFormSubmissionError}
            clearPortfolioToEdit={this.clearPortfolioToEdit}
            portfolioToEdit={this.state.portfolioToEdit} // <--- data 
          />
        </div>
```

Now in the **portfolio-form.js** file we're going to create a new function that will serve as a lifehook. We'll call this below the bindings. This lifehook will check if an object has keys or not. In JS, this is done by typing `òbject.keys(and the actual object)`, but we'll do this with a conditional.

```
componentDidUpdate() {
  if (Object.keys(this.props.portfolioToEdit).length > 0) {

  }
}
```

If the length is > 0 that means that a user clicked right here on the edit button. That's going to trigger ``componentDidUpdate`` because we are going to populate that prop and we're going to pass in that data so we're checking to see is there any data there. If so, we'll just grab all of the data and pass it to variables, and we're gonna use deconstruction in order to do that. 

```
componentDidUpdate() {
    if (Object.keys(this.props.portfolioToEdit).length > 0) {
      const {
        id,
        name,
        description,
        category,
        position,
        url,
        thumb_image_url,
        banner_image_url,
        logo_url
      } = this.props.portfolioToEdit;
  }
  }
```

 So what that's going to do is it's going to take this object that got passed in and it's going to spread it out. It's going to grab each element and it's gonna store it in a variable.

 However, every time a user is making a change such as typing in the form, the conditional is going to fire and clear everything out.

 To avoid this, we can call the ``clearPortfolioToEdit`` method inside this method. When this component changes is gonna skip the conditional 'cause it's gonna be empty already.

 ```
 componentDidUpdate() {
    if (Object.keys(this.props.portfolioToEdit).length > 0) {
      const {
        id,
        name,
        description,
        category,
        position,
        url,
        thumb_image_url,
        banner_image_url,
        logo_url
      } = this.props.portfolioToEdit;

      this.props.clearPortfolioToEdit()
    }
  }
  ```

  Now we'll call setState to populate it.

   We know we're always gonna have the ID and that is the first value that we have up here because every record has that, but not everything is gonna have a name or a description or a category so what we need to do is set up a conditional.

    So if there is a name it'll use the name, but if there's not a name it'll use an empty string and the way you can do that in JavaScript is by saying something like name and then we're gonna use the two pipes(|) and if you're not familiar with using the pipes, that is very similar. It's the or assignment operator or the or operator. So if there's data we'll use that, if not, we'll use an empty string.

    ```
    componentDidUpdate() {
    if (Object.keys(this.props.portfolioToEdit).length > 0) {
      const {
        id,
        name,
        description,
        category,
        position,
        url,
        thumb_image_url,
        banner_image_url,
        logo_url
      } = this.props.portfolioToEdit;

      this.props.clearPortfolioEdit

      this.setState({
        id: id,
        name: name || "",
        description: description || "",
        category: category || "eCommerce",
        position: position || "",
        url: url || ""
      });
    }
  }
  ```

  Now when clcking the edit button the form will show the item info, allowing us to edit its data.

## Making Dynamic Axios API Queries in React

So now when the user clicks on the edit button, they automatically populate the form, at least the text fields of the form, and all of that works perfectly. However, if you were to click save, it would not update the record. Instead, it would actually go and create a brand new record with all of those values which obviously is not what we want to occur so how can we fix that? Well, let's go down and see why it's still creating a record.

In the **portfolio-form.js** file, right below the **BuildForm** function, where it says handleSubmit, right now it's creating a post request, it's calling Axios.post and then it's calling the API endpoint to create a new record and take in all of that data from build form and that's exactly what it was meant to do, that's what we built it to do.

We can reconfigure the Axios request to create AND update data at the same time. Axios allows you to pass as the first argument, instead of being the url, a configuration object. That will allow to handle things dynamically. For example:

In the **portfolio-form.js** file we'll add a couple of items to the state:

```
this.state = {
      name: "",
      description: "",
      category: "Bussiness",
      position: "",
      url: "",
      thumb_image: "",
      banner_image: "",
      logo: "",
      editMode: false,
      apiUrl: "https://garazils.devcamp.space/portfolio/portfolio_items",
      apiAction: "post"
    };
```

> The API URL is the same as the handleSubmit's one.

Now, in the ComponentDidUpdate setState portion, we have to add the new items of the state, except for the edit mode, which is going to be true. The url is going to be the same except that we're going to use string interpolation:

```
componentDidUpdate() {
    if (Object.keys(this.props.portfolioToEdit).length > 0) {
      const {
        id,
        name,
        description,
        category,
        position,
        url,
        thumb_image_url,
        banner_image_url,
        logo_url,
      } = this.props.portfolioToEdit;

      this.props.clearPortfolioToEdit();

      this.setState( {
        id: id,
        name: name || "",
        description: description || "",
        category: category || "eCommerce",
        position: position || "",
        url: url || "",
        editMode: true,
        apiUrl: `https://garazils.devcamp.space/portfolio/portfolio_items/${id}`,
        apiAction: "patch"
      });
    }
  }
```
> The API verb **patch** creates an update action. Now when we click edit these values are going to be updated as well.

Now, inside the handleSubmit method, we'll get rid of the axios call only to call axios again. Instead of passing in a url we'll pass curly brackets. There we'll pass calls to the new state items from before:

```
 handleSubmit(event) {
    axios({
      method: this.state.apiAction, 
      url: this.state.apiUrl,
      data: this.buildForm(),
      withCredentials: true
    })
```

Now the items can be created and edited.

## Working with New and Edit Workflows in React

Now that we have our edit functionality working properly, it's time to connect the parent component with our form component, and what we're gonna do is build out two different workflows.

The form will have one workflow if it's a new form and another one if it's a edit one. We did it from an API perspective, now we have to do it from the app view itself.

In the **portfolio-form.js** file, we'll work on the handle submit.

right now the reason why if you go and you hit Submit in the form right now, it'll create a new record is because it is calling this method, this ``handleSuccessfulFormSubmission`` method. And this is fine, this works perfectly if it is a new record. But what we need to do is to be able to split this behavior off if it happens to be an edit type of submission.

Às a reminder, we have this now:

```
.then(response => {
        this.props.handleSuccessfulFormSubmission
        console.log("response", response);

        this.setState({
          name: "",
          description: "",
          category: "Bussiness",
          position: "",
          url: "",
          thumb_image: "",
          banner_image: "",
          logo: ""
        });
  ```

  What we'll do is we'll change that method name to reflect that that's the behavior of the new record, and then we'll create another function. So from ``handleSuccessfulFormSubmission`` we'll have ``handleNewFormSubmission``.

  > Every time you make a change like this, do a control version.

Now we'll work with the ``EditFormSubmission`` handler (we'll duplicate the line above and change its name), which we will now create. Inside of here we'll call the ``getPortfolioItems()`` method, because after we've edited the view, all we wanna do is have the system go through and repopulate all of the data because that's going to mimic exactly what happened, and we'll make sure that we're always working with the latest version of the data. An alternative approach would be for us to iterate through the collection of portfolio items. And then if we find the record with the ID, then we simply swap them out and that would be another way of doing it.

```
handleEditFormSubmission() {
    this.getPortfolioItems();
  }
```

Now we can pass this method as a prop to the form component inside the **portfolio-manager.js** file.

```
render() {
    return (
      <div className="portfolio-manager-wrapper">
        <div className="left-column">
          <PortfolioForm
            handleNewFormSubmission={this.handleNewFormSubmission}
            handleEditFormSubmission={this.handleEditFormSubmission}
            handleFormSubmissionError={this.handleFormSubmissionError}
            clearPortfolioToEdit={this.clearPortfolioToEdit}
            portfolioToEdit={this.state.portfolioToEdit}
          />
```

Now we need to know exactly when we want to call this edit process versus the handleNew and that's what we're gonna use our new condition of edit mode for. So inside of the response, and once again we're inside of handleSubmit here, we'll type this:

```
 handleSubmit(event) {
    axios({
      method: this.state.apiAction, 
      url: this.state.apiUrl,
      data: this.buildForm(),
      withCredentials: true
    })
      .then(response => {
        if (this.state.editMode) {
          this.props.handleEditFormSubmission();
        } else {
        this.props.handleNewFormSubmission(response.data.portfolio_item)
        }
```

This will trigger the edit method if the editMode is true, else, this is going to create a new record.

Now we need to update the state. we need to make sure that we're resetting it back to the correct apiUrl, to the correct action, and that's gonna get us back to where we want if a new record is gonna be submitted. So we'll grab the new items in the constructor and paste them inside the handleSubmit method.