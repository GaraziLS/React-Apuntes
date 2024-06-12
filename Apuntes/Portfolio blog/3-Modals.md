## Installing the React Modal Library

This is gonna be a relatively short guide and we're not gonna write a lot of code. Instead, we are going to plan out the next feature we're gonna build, which is the blog modal.

Modals are one of the standards types of page elements that most modern applications need to have in some form or another, so I thought this would be a good way of being able to create new blog posts.

**In short, modals are pop up windows that appear when you click on an icon. Forms are very usual Modals.** We'll use npm's`react-modal`to do this, so we'll first install it.

```
npm install --save react-modal
```

## Rendering the React Modal in the Blog Component

We'll call the modal component (which we'll create) from the **blog.js** file. The component will be in the `\src\components\modals\blog-modal.js` file.

We'll create a class component, import the ReactModal library, and call it inside the render function. This lbrary takes optional props for customization purposes, in this case we'll type `isOpen={true}` to make it to be open. Later on we'll close and open it with a button.

This is not a self-closing tag because all the code that goes inside gets passed in as a child component.

```
import React, { Component } from 'react';
import ReactModal from "react-modal";

export default class BlogModal extends Component {
    constructor(props) {
        super(props);

}
    render() {
        return (
        <div>
            <ReactModal isOpen={true}>
                <h1>Testing modals</h1>
            </ReactModal>
        </div>
        );
    };
}
```

Now we'll call it from the **blog.js** file and because the isOpen param is true, it will be displayed on the screen.

## Triggering the React Modal to Open with a Link Click

In this guide is create a link that has a click listener and then, when we click on that link, it will then open up the modal. We're not gonna worry about closing it, or submitting anything, or anything like that yet. All I care about is passing props directly to the modal to let it know when it should open up.

We're going to open the **blog-modal.js** file and pass props to the isOpen prop. We haven't created it yet, though:

```
render() {
        return (
        <div>
            <ReactModal isOpen={this.props.modalIsOpen}>
                <h1>Testing modals</h1>
            </ReactModal>
        </div>
        );
    };
}
```

We'll open the blog.js file because it needs to know about the state of the blog: is it open, or closed? To do that, we'll create another item inside the state.

```
class Blog extends Component {
  constructor() {
    super();

    this.state = {
      blogItems: [],
      totalCount: 0,
      currentPage: 0,
      isLoading: true,
      blogModalIsOpen: false
    };
```

It is set to false because we don't want it to inmediately show it, just when a button is clicked. Now we'll see how to update the state, so we need a new handler.

```
handleNewBlogClick() {
    this.setState({
      blogModalIsOpen: true
    })

  }
```

Now we'll go down to the ``BlogModal`` component to pass a prop (the same that we created in the ``blogModal`` file) with this handler inside:

```
render() {
const blogRecords = this.state.blogItems.map(blogItem => {
return <BlogItem key={blogItem.id} blogItem={blogItem} />;
});

    return (
      <div className="blog-container">
        <BlogModal modalIsOpen={this.state.blogModalIsOpen}/>

```

When the component first loads, the state is going to be false, so the modal won't be showing up.

Now we'll add a div with a link and pass in the click handler.

```
return (
<div className="blog-container">
<BlogModal modalIsOpen={this.state.blogModalIsOpen} />

        <div className="new-blog-link">
          <a onClick={this.handleNewBlogClick}>Open Modal</a>
        </div>
```

Now when clicking on that link the modal will open.`

**In review**, we first channged the value of ``isOpen`` in the **ReactModal** Component by passing props.

```
render() {
        return (
        <div>
            <ReactModal isOpen={this.props.modalIsOpen}>
                <h1>Testing modals</h1>
            </ReactModal>
        </div>
        );
    };
}
```

Then the ``BlogModal`` component inside the **blog.js** file receives the prop, that carries the state:

```
<BlogModal modalIsOpen={this.state.blogModalIsOpen} />
```


Then, inside of the **blog.js** file we added a base style to tell the system that the modal is closed whenever the file first loads. Then, we created a handler with the value of true:

```
this.state = {
      blogItems: [],
      totalCount: 0,
      currentPage: 0,
      isLoading: true,
      blogModalIsOpen: false

(...)

handleNewBlogClick() {
    this.setState({
      blogModalIsOpen: true
    })
```

This handler is passed down to the ``BlogModal`` component`to trigger on a click:

```
<div className="new-blog-link">
          <a onClick={this.handleNewBlogClick}>Open Modal</a>
        </div>
```

## How to Close the React Modal

Now that we have the ability to open the Modal by simply clicking on it, we now need the ability to close it. We will do it by either clicking a button or by clicking outside the modal box.

We'll start by, in the **blog-modal.js** file, passing props to the component.

```
<ReactModal onRequestClose={() => {
  console.log("testing modal close")
            }}
```

``OnRequestClose`` and ``isOpen`` differ because the latter expects a boolean value, true or false. OnRequest, however, expects a function.

Now by clicking outside the box modal the console.log triggers because that's already pre-built in the library. It also works if we hit escape.

Now we'll build the function itself.

```
<ReactModal onRequestClose={() => {
                this.props.handleModalClose();
            }} isOpen={this.props.modalIsOpen}>
                <h1>Testing modals</h1>
            </ReactModal>
        </div>
        );
    };
}
```

> We'll build the function in the **blog.js** file. Also note the parenthesis, we call the function with them because we're calling that function directly. **Because this is an actual function that we want to fire** that's a reason why we have to **call it with parenthesis** whereas **whenever we're passing in a method that's simply a callback function, we don't have the parenthesis** in there but **whenever you call a prop as a function and it's inside of an anonymous function that's when you're gonna want to put the parenthesis in there.**

So now we'll build the function and set state:

```
handleModalClose() {
    this.setState({
      blogModalIsOpen: false
    })
  }
```

And now we can pass the function as a prop to the component in **blog.js** file:

```
<BlogModal handleModalClose={this.handleModalClose} modalIsOpen={this.state.blogModalIsOpen} />
```

> We call the **function without parenthesis**.

Now the modal works and we can add styles. 

## Adding styles to the modal

For modals, the styles are usually put inline. In the **blog-modal.js** file we'll create a function inside the constructor, the styles will go inside.

> The class names are their own object and you can pass custom styles to those objects. We'll work with the content and overlay classes (all of this is in the react-modal documentation).

```
this.customStyles = {
    content: {
        top: "50%",
        left: "50%",
        right: "auto",
        marginRight: "-50%",
        transform: "translate(-50%, -50%)",
        width: "800px"
    },

    overlay: {
        backgroundColor: "rgba(1, 1, 1, 75%)"
            }
  }
```

Now we'll pass that as a prop to the ReactModal component in the **blog-modal.js** file:

```
 <ReactModal 
      style={this.customStyles}
      onRequestClose={() => {
      this.props.handleModalClose();
  }} 
  isOpen={this.props.modalIsOpen}>
```

## Fixing Screen Reading Warning for React Modal

If you open up the JavaScript console, and you click on Open Modal, you'll see this ugly little warning:

```
warning.js:34 Warning: react-modal: App element is not defined. Please use `Modal.setAppElement(el)` or set `appElement={el}`. This is needed so screen readers don't see main content when modal is opened. It is not recommended, but you can opt-out by setting `ariaHideApp={false}`.
```

In the blog-modal,js file, right above the class definition, we'll type this to fix the issue:

```
ReactModal.setAppElement(".app-wrapper")
```

And we need to pass that element (in this case, a wrapper for the entire application). It can be found in the **index.html** file of the src > static folder. There we will find the app-wrapper class.

Now the issue is gone.



