## Building a Delete Click Handler in React

We'll now build the functionality to edit and delete data, as well as the CRUD functionality (it stands for Create, Read, Update and Delete).

We want to add the ability of deleting records from the sidebar and doing so the sidebar would update as well.

We're going to the **portfolio-sidebar-list.js.** and type this code in the a tag (it's a link that when clicked triggers the delete function. That function calls each item in the collection):

```
<h1 className='title'>{PortfolioItem.name}</h1>
   <h2>{PortfolioItem.id}</h2>
    <a onClick={() => props.handleDeleteClick(PortfolioItem)}>Delete</a>
```

We'll create the handler itself in the **portfolio-manager.js** file we'll create and bind the handler as usual. Don't forget to pass the handler into the Sidebar component calling, because those props are already in the portfolio-sidebar-list.

```
<PortfolioSidebarList data={this.state.portfolioItems} handleDeleteClick={this.handleDeleteClick}/>
```

## How to Delete API Data in React and Update State

Now that we have a handler built, that is listening for any user to click on Delete, we now have the ability to call the outside API, pass in the delete verb and then have it remove that record from the database.

In the portfolio-manager.js file, we'll call axios inside the Delete handler and the url, and this is where we want to pass in the actual record so we're going to use our string interpolation.

After the url we're going to say dollar sign curly brackets and then inside of here, pass in portfolioItem which we're getting from the deleteClick handler and then we want the ID for that, so the url will end with the ID and the API is going to delete that. Then we're going to pass in the WitchCredentials to ensure our identity.

```
 handleDeleteClick(portfolioItem) {
    axios.delete(
      `https://api.devcamp.space/portfolio/portfolio_items/${portfolioItem.id}`,
      { withCredentials: true }
    )
    
  }
```

Now we'll build the promise.

```
handleDeleteClick(portfolioItem) {
  axios
    .delete(
      `https://api.devcamp.space/portfolio/portfolio_items/${portfolioItem.id}`,
      { withCredentials: true }
    )
    .then(response => {
      console.log("response from delete", response);
    })
    .catch(error => {
      console.log("handleDeleteClick error", error);
    });
}
```

Now we have to remove that record from the state. When the response comes back, we're going to iterate over the state items and remove that record from the state. Inside of the delete handler's promise, we'll set state. Inside the state that we're setting, we're actually going to iterate and build a new collection on the fly and we're gonna use the filter function to do that.

```
handleDeleteClick(portfolioItem) {
    axios
      .delete(
        `https://api.devcamp.space/portfolio/portfolio_items/${portfolioItem.id}`,
        { withCredentials: true }
      )
      .then(response => {
        this.setState({
          portfolioItems: this.state.portfolioItems.filter(item => {
            return item.id !== portfolioItem.id;
          })
        });
```

So the filter grabs only the items we want and then in the return statement we wanna say that as you iterate through if you find a record and the item.id is not equal to the ID right here that we want to delete, then I want you to return that meaning that I want you to keep those records but I want you to get rid of the one record that has that ID. 

