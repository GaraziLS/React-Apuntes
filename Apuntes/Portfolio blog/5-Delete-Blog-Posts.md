## Enabling the Ability to Delete Blog Posts

In this guide we are going to implement the ability to delete blog items from our blog page. We want to delete the posts only when we're logged in.

We need to create a slightly different BlogItem. In the **blog.js** file, right now, if you scroll down into what's getting returned, you'll see that we have these blogRecords, and it's simply iterating over your state and it is saying okay, here is our BlogItem component. And that works perfectly fine, and that's the behavior we wanna keep for users that are not logged in.

But if you are logged in we want to create a slightly different value that gets returned.

```
render() {
    const blogRecords = this.state.blogItems.map(blogItem => {
      return <BlogItem key={blogItem.id} blogItem={blogItem} />;
    });
```

So we're going to add a conditional to display a process when the user is logged in, and another one when it's not.

```
 render() {
  const blogRecords = this.state.blogItems.map(blogItem => {
    if (this.props.loggedInStatus === "LOGGED_IN") {
      return (
        <div key={blogItem.id} className="admin-blog-wrapper">
          <BlogItem blogItem={blogItem} />
          <a onClick={() => this.handleDeleteClick(blogItem)}>Delete</a>
        </div>
      );
    } else {
      return <BlogItem key={blogItem.id} blogItem={blogItem} />;
    }
  });
```

> Note that the return statement with the blog item is moved. We'll create the handler of the <a> link later on. Include a key in the most outer div, or a warning will pop up in the console.

Now a link appears below each blog post when we're logged in.

The handleDeleteClick needs as an argument the blog item that's going to be deleted, but if we do that on the <a> tag it would be executed immediately, it would not wait for the click, so we need to pass curly brackets.

> Whenever you want to pass a function that must wait for an event, use anonymus functions and curly brackets.

```
<a onClick={() => {this.handleDeleteClick(blogItem)}}>Delete</a>
```

Now we can go to the handleDeleteClick handler and pass a blog argument:

```
handleDeleteClick(blog) {
    console.log("deleted post", blog)
  }
```

Now if we click on the delete link the console has the item itself. We'll need the ID tobe passed into the API so the API knows which item is going to be deleted. Now we can update that handler by calling axios:

```
handleDeleteClick(blog) {
  axios
    .delete(
      `https://api.devcamp.space/portfolio/portfolio_blogs/${blog.id}`,
      { withCredentials: true }
    )
    .then(response => {
      console.log("res from delete", response);
    })
    .catch(error => {
      console.log("delete blog error", error);
    });
}
```

> If when deleting a post you get a 204 status, that means the deletion was successful.

We now need to update state to actually delete blog posts, so we'll update the promise.

```
.then(response => {
        this.setState({
          blogItems: this.state.blogItems.filter(blogItem => {
            return blog.id !== blogItem.id;
          })
        });

        return response.data;
      })
```

What this is doing is with this we have our blogItems inside of state. And we're going to set this equal to this.state.blogItems.filter, and we're going to have access to each one of the blogItems in our state. Remember, that's how a filter works. Filter's very similar to map, it simply gives you the ability to pick and choose which values you wanna add into the array that you're creating.

So I'm gonna say return if the blog.id is not equal to the blogItem.id. So all we're doing here, just kind of as a review, is we're iterating through each one of the blogItems, and we're saying if the blogItem we're currently looking at, so let's say that we have this full list like we do right here, and it's gonna look at testing after update feature, then it's gonna look at rich text image.

It's gonna look at each one of these in state. And then it's gonna compare that with the ID of whatever we clicked on for delete. If it does not line up, it'll keep it, if it does line up it's going to just be left out, which is exactly the behavior we want.

Now the delete links work.