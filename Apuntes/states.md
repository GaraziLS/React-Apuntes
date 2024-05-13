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

Then we update the *portfolioItems()* function to call this state (the key is needed inside the prop because the function now refers to the state), and the loop will still work.

>  To avoid confusions with this.state we can do this:

```
render() {
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

## Deep dive 1: states, props and components

Project syntax:

We have an app file that has two components inside, *JournalList*, with a **prop** called **heading** each. This component is in a separate file called *JournalList* as well, and is in fact imported.

```
import React, { Component } from 'react';
import JournalList from "./journals/journal_list"


export default class App extends Component {
  render() {
    return (
    <div>
      <h1>React, Props and State deep dive</h1>
      <JournalList heading = "List 1"/>  
      <JournalList heading = "List 2"/>  
    </div>
    );
  };
}
```

In the *JournalList* file (the code from here won't be explained in order, but chronologically to understand it better) we first have (react import aside) the **JournalList** class. This has a constructor with the prop that will render the app.js file component. 

```
export default class JournalList extends Component {
    constructor(props) {
        super();

        this.state = {
            data: JournalData,
            isOpen: true
        }
    }
    render() {
        return (
            <div>
            <h2>{this.props.heading}</h2>
            <JournalEntry title = "Lorem ipsum" content = "blablabla"/>
            </div>
        );
    };
}
```

It also has a state with an associated constant where it pulls its data from, but it's written right below the imports:

```
const JournalData = [
    {title: "Post 1", content: "Lorem ipsum", status: "draft"},
    {title: "Post 2", content: "Lorem ipsum", status: "published"},
    {title: "Post 3", content: "Lorem ipsum", status: "published"}
]
```

Then comes the render method, that has the app file's components' prop called (the heading) and below the component of JournalEntry that has two props, thus it's nested to JournalList. 

```
render() {
        return (
            <div>
            <h2>{this.props.heading}</h2>
            <JournalEntry title = "Lorem ipsum" content = "blablabla"/>
            </div>
        );
    };

```

The *JournalEntry* component is defined below the *JournalData* array and it's a functional component, and has the *JournalEntry's* props passed on (this component will get its own separated file):

```
const JournalEntry = (props) => {
    return(
        <div>
            <h1>{props.title}</h1>
            <p>{props.content}</p>
        </div>
    )
}
```

### Changing components dynamically

 We'll now create, inside the render function and before the return, a constant that will store the data state to change it dynamically.

```
render() {
        const journalEntries = this.state.data
        return (...)
}
```

Then we're gonna use the map function to loop over entries.

```
render() {
        const journalEntries = this.state.data.map(entry => {
            return(
                <div key={entry.title}></div>
            )
        })
}
```
> The *key=* inside the div mimics a unique id that would come from an api. 

Inside of this loop we'll call the component and pass in the data tags (cause we're iterating through data state) to dynamically change the content (which is stored in the data variable) instead of hardcoding it.

```
render() {
        const journalEntries = this.state.data.map(entry => {
            return(
                <div key={entry.title}>
                    <JournalEntry title = {entry.title} content = {entry.content}/>
                </div>
            )
        })
        return (...)
}
```

Now we can call the JournalEntries component:

```
export default class JournalList extends Component {
    constructor(props) {
        super();

        this.state = {
            data: JournalData,
            isOpen: true
        }
    }
    render() {
        const journalEntries = this.state.data.map(entry => {
            return(
                <div key={entry.title}>
                    <JournalEntry title = {entry.title} content = {entry.content}/>
                </div>
            )
        })
        return (
            <div>
            <h2>{this.props.heading}</h2>
            {journalEntries}
            </div>
        );
    };
}
```

### Manipulating state

We'll now create a button that changes the state. To do so, below the constructor we'll create a function:

```
export default class JournalList extends Component {
    constructor(props) {
        super();

        this.state = {
            data: JournalData,
            isOpen: true
        }
    }

    ClearEntries = () => {
        this.setState({data: []})
    }

    render() {...}
}
```

This empties the data variable, but by itself won't do anything. Now we're gonna create the button inside the render function. 

```
return (
            <div>
            <h2>{this.props.heading}</h2>
            {journalEntries}
            <button onClick={this.ClearEntries}>Clear Journal entries</button>
            </div>
        );
```

> The function goes without () because it's stored inside a variable.

We'll keep changing the state by adding more state tags to the function and by creating another method that shows data instead. This one will also have a button:

```
ShowEntries = () => {
        this.setState({data: JournalData, isOpen: true})
    }
```

Finally, we'll add another method to switch between the previous two buttons:

```
ToggleEntries = () => {
        if(this.state.isOpen) {
            this.setState({JournalData: [], isOpen: false})
        } else {
            this.setState({JournalData: JournalData, isOpen: true})
        }
    }
```