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