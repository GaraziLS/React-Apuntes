## Creating a data filter

We're going to create a data filter. To do that, we're going to create a function that handles this, including its bind to *this*. It's a best practice to name "handle" functions that are going to handle events.

So far, we'll have to have this:

```
handleFilter(filter) {
        this.setState({

        })
    }
```
And now we're gonna loop over the data keys. We've added one more:

```
constructor() {
        super(); 
        this.state = {
            pageTitle: "Welcome to my portfolio",
             data: [
                 {title: "First", category: "entertainment"},
                  {title: "Second", category: "eCommerce"},
                  {title: "third", category: "personal"}
                 ]
            }
        };
```

To create the filter we'll go to the *setState* inside the function. 

> *Filter allows you to pick a specified item from a collection of data. What filter does is it allows you to loop through the collection, you pass in a conditional. If that condition is true then it adds that entire element directly into wherever you're storing it, which in this case is this selected variable.

 ![filters](https://s3-us-west-2.amazonaws.com/images-devcamp/Dissecting+React+JS/React+JS+fundamentals/How+to+Build+a+Data+Filter+in+React+%23+2355/image12.png)*

```
handleFilter(filter) {
        this.setState({
            data: this.state.data.filter(item => {
                return item.category === filter
            })
        })
    }
```

Inside the *this.state* we select *data* and pass over *filter* to it. Inside it, we loop normally and return the looping key + the object key if its === to the filter.

Now we're gonna create buttons inside the render function, and the event listener will be quite different because if we try to pass a normal function issues will happen. To avoid that we'll use an anonymus function and then will call *this.function("object value"):*

```render() {
        return (
            <div>
                <h2>{this.state.pageTitle}</h2>

                <button onClick={() => this.handleFilter("eCommerce")}>eCommerce</button>
                <button onClick={ () => this.handleFilter("Personal")}>Personal</button>
                <button onClick={() => this.handleFilter("Hobby")}>Hobby</button>

                {this.portfolioItems()}
            </div>
        )
    }
```

However, all of this only work once because when clicking on other buttons the content disappears. This happens because the state changes completely and it ignores the other buttons. We'll fix this later.

## Full Filtering Feature Build Out in React

Now, we'll build the full Filter feature. Until now, when we clicked a second time on a filter, the screen goes blank. We want to reset the filter once it's filtered to view all the items again and to re-apply the filter again.

First, we're going to create another button that clears the filter. Second, we're going to work on the getPortfolioItem to be dynamic so in some cases it returns the filter items and in others all the items.

We'll start in the **portfolio-container.js** file. After wrapping all the buttons inside some divs, we'll add another button to clear all the filters.

After giving some styles, we'll come to the handleFilter function to add a conditional.

```
handleFilter(filter) {
    if (filter === "CLEAR_FILTERS") {
      this.getPortfolioItems();
    } else {
      this.setState({
        data: this.state.data.filter(item => {
          return item.category === filter;
        })
      });
    }
  }
```

This is calling the getPortfolioItems function, which calls the API and brings in all the data. Thus, the filter is cleared if we click the All tab.

That's the first part of it, because if we click a second time on a filter the screen goes blank.

To fix this, we'll add an argument into the <u>getPortfolioItems</u> method:

```
getPortfolioItems(filter = null)
```

This allows us to,  in JavaScript, what this allows us to do is that ***this is saying that filter is optional***. You don't have to pass one in but if you do, the function can actually work with it.

So we will add more conditionals.

```
getPortfolioItems(filter = null) {
    axios
      .get("https://garazils.devcamp.space/portfolio/portfolio_items")
      .then(response => {
        if (filter) {
          this.setState({
            data: response.data.portfolio_items
          });
        } else {
          this.setState({
            data: response.data.portfolio_items
          });
        }
```

So first we pass in the data in the two parts of the conditional. Second, in the handle filter method

```
handleFilter(filter) {
    if (filter === "CLEAR_FILTERS") {
      this.getPortfolioItems();
    } else {
      this.setState({
        data: this.state.data.filter(item => {
          return item.category === filter;
        })
```

we can ***cut the .filter*** portion and paste it in the <u>getPortfolioItems</u> method, in the if. Thus, if there's a filter, the loop will work, because we're iterating over the data:

```
 getPortfolioItems(filter = null) {
    axios
      .get("https://garazils.devcamp.space/portfolio/portfolio_items")
      .then(response => {
        if (filter) {
          this.setState({
            data: response.data.portfolio_items.filter(item => {
              return item.category === filter;
            })
          });
        } else {
          this.setState({
            data: response.data.portfolio_items
          });
        }
      })
```

Now inside the handleFilter method we can cut the setState and do this:

```
handleFilter(filter) {
    if (filter === "CLEAR_FILTERS") {
      this.getPortfolioItems();
    } else {
      this.getPortfolioItems(filter)
    }
  }
```

So the getPortfolioItems will behave like there's no filter (thus returning the content) and like there is one (thus it will filter items).

Now the filter is fully working.