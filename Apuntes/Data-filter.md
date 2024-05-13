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

