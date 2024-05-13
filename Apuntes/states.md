## States

An state is the state of your application, or I should say, the state of your component.

Your component has the ability, as long as it's a **class component**, it has the ability to manage its own state, and that's usually related to data in some form or another.

Let's get the Portfolio Items function from the container.

```
portfolioItems() {
        const data = ["First", "Second", "Third"];
        return data.map(_item => {
            return <PortfolioItem title={_item} url={"www.google.es"} />; // component and prop with _item inside
        })
    }
```

Instead of hadcoding data we're going to create a state. States inside the constructor function are called initital states because this is what the component will get whenever is called.

To create a state, type (**INSIDE THE CONSTRUCTOR**)

```
this.state = 
```

And then a key-value pair object:

```export default class PortfolioContainer extends Component {
    constructor() {
        super(); 
        this.state = {
            pageTitle: "Welcome to my portfolio",
            
        };
    }
}
```

Then it's called whenever is needed with *{this.state.Object key name}*

We can add more states, such as the data array with its content nested:

```
export default class PortfolioContainer extends Component {
    constructor() {
        super(); 
            this.state = {
                pageTitle: "Welcome to my portfolio", // <--- Initial state
                data: [
                    {title: "First"}, 
                    {title: "Second"},
                    {title: "Third"} 
                ]
            };
    }
    portfolioItems() {
        return this.state.data.map(_item => {
            return <PortfolioItem title={_item.title} url={"www.google.es"} />; // component and prop with _item inside
        })
    }}

 ```

Then we update the *portfolioItems()* function to call this state (the array key is needed inside the prop because the function now refers to the state), and the loop will still work.

>  To avoid confusions with this.state we can do this:

```
render() {}
    const {ComponentName, AnotherComp} = this.state
}
```

This way we can call it by name, as if it was a normal variable (remember to add the curly brackets inside render() if JS code is written)

```
render(...) {
   } return <h1>{ComponentName}</h1>
```

Object deconstruction can be performed as well:

```
render(...) {
   } return <h1>{ComponentName}</h1>
   <h2>{AnotherComp}</h2>
```

> How to set up a timer that updates automatically:
>```
>componentDidMount() {
>  setInterval(() => {
>    this.setState({ currentTime: String(new Date())});
>  }, 1000);
>}
>```

You can also store this in a variable:

```
componentDidMount() {
  this.liveTime = setInterval(() => {
    console.log("New chat message");

    this.setState({ currentTime: String(new Date())});
  }, 1000);
}
```

You should clean things from time to time to avoid performance issues:

```
componentWillUnmount() {
  clearInterval(this.liveTime);
}
```

### Changing state values

We use *this.setState* for this. Since SetState is a method, () are needed, and inside them an object must be placed.

```
 HandlePageTitleUpdate() {
        this.setState({
            PageTitle: "Something else"
        })
    }
```

Now the page title will be updated, but to use it we must use a click handler. By clicking on a button, react will listen to that event.

```
<button onClick={this.HandlePageTitleUpdate}>Change title</button>
 ```
> JSX writes the event differently: the onClick event listener must be written between {}. Inside them, a function must be called **without the ()** in order to work. 

**If a "Cannot read setState of undefined" error pops up**, we need to let the component know that it should have access to the keyword "this." So it needs to know that it can have access to all the data for the component, and the way that we can do that is to come inside of the constructor and type: 

```
this.HandlePageTitleUpdate = this.HandlePageTitleUpdate.bind(this);
```

So what we're doing here is we're saying that this function here needs to be tied and needs to be bound to this instance of the component. So this *PortfolioContainer*, whenever it is created and rendered on the page, we need to bind *handlePageTitleUpdate* so that it has access to all of this data, such as the page title, so that we can connect it and call *this.handlePageTitleUpdate*. 

> **In short, custom functions that handle events need to have a *this* bound, and that can be done this way:**

```
this.FunctionNameWithout() = this.FunctionNameWithout().bind(this)
```