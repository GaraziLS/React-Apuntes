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

    Then we update the *portfolioItems()* function to call this state (the key is needed inside the prop because the function's prop now refers to the state), and the loop will still work.