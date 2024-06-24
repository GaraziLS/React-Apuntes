## Enabling Authorization Rules for Blog Detail Component

This section of the course is gonna be all about cleanup.

In this guide we're going to add a security fix. Right now if you click on an item you can edit the form without being logged in.

We'll start at the **app.js** file. We're going down to the route with the slug and add a render prop. We can remove the component prop:

```
<Route path="/b/:slug" />
    {this.state.loggedInStatus === "LOGGED_IN" ? (
        this.authorizedPages()
              ) : null}
```

Now we'll pass the render prop:

```
<Route 
  path="/b/:slug" 
  render={props => (
    <BlogDetail {... props} loggedInStatus={this.state.loggedInStatus}/>
  )}
  />
  {this.state.loggedInStatus === "LOGGED_IN" ? (
    this.authorizedPages()
  ) : null}
```

Now inside the **blog-detal.js** page we can access the LOGGED_IN status.

In that file, we'll go to the content manager. Right now we're only checking for an edit mode, but we can add more conditionals to only return the blog form if the edit mode AND the logged in status are active at the same time:

```
const contentManager = () => {
      if (this.state.editMode && this.props.loggedInStatus === "LOGGED_IN") {
        return <BlogForm>
```

Now if we clck on blog titles the form doesn't appear.

The other option is going to the ``handleEditClick`` method and put that check there.

```
handleEditClick() {
  if (this.props.loggedInStatus === "LOGGED_IN") {
    this.setState({ editMode: true });
  }
}
```