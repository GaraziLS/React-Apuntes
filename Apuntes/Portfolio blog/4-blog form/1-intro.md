## Creating the Initial Blog Form Component

We're going to create a forn tha's going to be inside a modal. Inside the **src > components > blogs**, we're going to create a new file called **blog-form**. This is the component that will be inside the modal. So we'll create it and export it to the *blog-modal.js* file. Then we'll call it inside the *ReactModal* component.

## Building the Blog Form Event Handlers

We're going to build a couple of things: one, our form, whenever a user is typing into it, we want that to update the form's local state. Then from there when the form is submitted we wanna wrap up all of that data and then let the parent component, which in this case is the blog modal, we wanna let it know that the form was submitted.

We're not gonna connect to the API yet or anything like that. Right here, we're simply focusing on state management and on data flow, and being able to have two components that can pass data between themselves.

**To get started with this**, inside the **blog-modal.js** component, we'll add inside the BlogModal class the function that will be passed on as a prop whenever the form is submitted successfully to the BlogForm component.

```
 overlay: {
                backgroundColor: "rgba(1, 1, 1, 75%)"
            }
        }

        this.handleSuccessfullFormSubmission = this. handleSuccessfullFormSubmission.bind(this)
    }

    handleSuccessfullFormSubmission(blog) {
        console.log("blog from blog form that comes through the API", blog)

    (...)

     <BlogForm handleSuccessfullFormSubmission={this.handleSuccessfullFormSubmission}/>
```

In the **blog-form.js** file we'll set a base state because we need to update the component state everytime the form is changed.

```
export default class BlogForm extends Component {
    constructor(props) {
        super(props);

        this.state = {
            title: "",
            blog_status: ""
        }

        this.handleChange = this.handleChange.bind(this)
}

        handleChange(event) {
            console.log("handleChange", event)
        }
```

Now we'll pass that handler to the input:

```
 <input onChange={this.handleChange} type='text'></input>
```

Now we can type in the first input to check everything is right, so we can delete the console.log statement and set state directly.

> Test things often via console logs to avoid bugs.

The state will be dynamic because sometimes the change will affect the title and other times the blog status. Whenever you need things to be dynamic or if you need a key be passed as a variable, you just wrap it in square brackets.

```
handleChange(event) {
            this.setState({
                [event.target.name]: event.target.value
            })
        }
```

Now we need to update the inputs.

```
<input 
        onChange={this.handleChange} 
        type='text'
        name="title"
        placeholder='Title'
        value={this.state.title}>
    </input>
    <input 
         onChange={this.handleChange} 
         type='text'
         name="blog_status"
         placeholder='Blog status'
         value={this.state.blog_status}
    ></input>
```

> The ``name`` attribute is important because that's where the ``event.target.name`` comes from. The ``name`` must match the state value and the value is dynamic.

Now we'll add the form submission handler. 

```
this.handleSubmit = this.handleSubmit.bind(this)
}

        handleSubmit(event) {
            this.props.handleSuccessfullFormSubmission(this.state)
            event.preventDefault()
        }

        (...)

        <form onSubmit={this.handleSubmit}>
```

> The ``handleSuccessfullFormSubmission`` methos is already passed in the ``blog-modal.js`` file.

The handleSuccessfullFormSubmission(this.state) will get the API petition later.

Now if we type something and hit save the console will log the response.

## Creating Blog Posts via the API

In this guide, we are going to make it possible to create a blog record from our blog from and we are only going to be passing these two values, the Title and the Blog status for now but that's all we need in order to build out our API connection.

In the **blog-form.js** file we're going to create a method called **buildForm()**. Inside that we're going to build a FormData object, and then are going to append things.

```
 buildForm() {
            let formData = new FormData();

            formData.append("portfolio_blog[title]", this.state.title);
            formData.append("portfolio_blog[blog_status]", this.state.blog_status);

            return formData
        }

```

Now we're going to call axios inside the handleSubmit method.

```
handleSubmit(event) {
            axios.post(
                "https://garazils.devcamp.space/portfolio/portfolio-blogs",
            this.buildForm(),
            { withCredentials: true}
        )
        this.props.handleSuccessfullFormSubmission(this.state);
        event.preventDefault();
    }
```

After the url, the next argument is where we're passing our data, in this case, the buildForm method. And then we're passing the withCredentials: True because this is a blog form that wi'll be written when we are logged in.

From there, we will build the promise. We need to tell axios what to do when that response comes back, so I'm gonna say then response and then pass in a function and for right now let's just console.log it but we're not gonna console.log in the handleSubmit, remember, that's exactly what our prop ``(handleSuccessfullFormSubmission)`` does. Instead of the this.state, we're going to pass the response.

```
 handleSubmit(event) {
            axios.post(
                "https://garazils.devcamp.space/portfolio/portfolio_blogs",
                this.buildForm(),
                { withCredentials: true}
            ).then(response => {
                this.props.handleSuccessfullFormSubmission(response.data);
            })
            .catch(error => {
                console.log("handleSubmit for blog error", error);
              });
```

Now when we submit the form the console responds, and records are updated in the API.

## Building the Full Blog Creation Workflow

So the goal by the end of this guide is to be able to click on our Open Modal link, to be able to create a guide by using the form, and then when we hit save I want the form to clear out, I want the modal to close, and I want that new blog post to be placed at the very top of the page.

We'll start in the **blog-form.js** file (child of *blog-modal.js*). In order to clear the form we need to, inside the ``handleSubmit`` method, set state after the then, because after the then the data will be submitted and we want to clear the form once it's submitted.

The value is pulled from the state value.

We're already communicated with the parent component ``handleSuccessfullFormSubmission, that method is estabished in the **blog-modal.js** file)``. We'll also update that method to send the record directly to the **Blog-modal.js** file.

```
this.props.handleSuccessfullFormSubmission(response.data.portfolio_blog)
```

In the **blog-modal.js** file we're going to work with the ``handleSuccessfullFormSubmission`` method. We'll first remove the console.log.

```
handleSuccessfullFormSubmission(blog) {
        this.props.handleSuccessfulNewBlogSubmission(blog)
    }
```
We'll going to bind this in the **blog.js** component, and create this handler there (the method comes from the blog-modal.js, so binding it into the *blog.js* file means that blog is the grandparent of *blog-modal*.). We'll set state to close the modal once the form is submitted, and we also want to take the blog record and pass that directly into our list of blogs, and at the very top of the list.

```
handleSuccessfulNewBlogSubmission(blog) {
    this.setState({
      blogModalIsOpen: false,
      blogItems: [blog]
    })
  }
```

This way we close the modal once the form is submitted.

The easiest way to take the blog record and pass that directly into our list of blogs, and at the very top of the list is to create an array with a single item, the blog itself. From there, we're going to concat the blog items to get the blog itself before the others:

```
handleSuccessfulNewBlogSubmission(blog) {
    this.setState({
      blogModalIsOpen: false,
      blogItems: [blog].concat(this.state.blogItems)
    })
  }
```

Now we need to pass that method to the ``BlogModal`` that's in the **blog-form.js** file.

```
<div className="blog-container">
        <BlogModal 
        
        handleSuccessfulNewBlogSubmission={this.handleSuccessfulNewBlogSubmission}
```

Now we'll add styles to the form.


## Building the Styles for the New Blog Icon with a Fixed Position on the Page

We'll import an icon to create a new blog from there. We'll import the icon from the **app.js** file. Then in the **blog.js** file we'll bring the icon in in place of the *Open Modal* text link:

```
<div className="new-blog-link">
          <a onClick={this.handleNewBlogClick}><FontAwesomeIcon icon="circle-plus"></FontAwesomeIcon></a>
        </div>
```

Now we can add styles.

## Revisiting Render Props and Passing the Logged In Status to Child Components

We have the blog creation button, but it's still there when we log out. This can lead to issues because when logged out we don't want to create blogs. The system itself would actually throw an error because it wouldn't recognize the user and so it wouldn't let the records to be created anyway, so this little button here would just be confusing.

We are going to tap into our state's logged in status, the state for the entire application, the parent component and we're gonna see how we can pass that state down using render props directly into the blog component.

In the **app.js** file we already passed render props to the auth route. We're going to replicate that for the blog one.

However, the auth render props carry () instead of {}, which the blog route have. Now the latter would throw an error. That's because how explicit and implicit returns work. If you use {} you must use the *return* reserved word. However, you can skip the return if you type the entire function in one line or if you use () (this only works with JSX).

So we get this: 

```
<Route
   path="/blog"
   render={props => (
     <Blog {...props} loggedInStatus={this.state.loggedInStatus} />
   )}
 />
```

Now we can use a ternary operator to hide the new blog creation link, which is in the **blog.js** file.

```
{this.props.loggedInStatus === "LOGGED_IN" ? (
          <div className="new-blog-link">
            <a onClick={this.handleNewBlogClick}>
              <FontAwesomeIcon icon="plus-circle" />
            </a>
          </div>
        ) : null}
```

Now the icon disappears when we log out.

## Building a Dedicated Icon Helper File in React

We will refactor all the app to pull out the icon calls from the **app.js** file. In the src folder we'll create another folder called helpers.

```
import {
    faTrash,
    faSignOutAlt,
    faEdit,
    faSpinner,
    faPlusCircle
  } from "@fortawesome/free-solid-svg-icons";
  import { library } from "@fortawesome/fontawesome-svg-core";

  const Icons = () => {
  return library.add(faTrash, faSignOutAlt, faEdit, faSpinner, faPlusCircle); }

  export default Icons;
  ```

  Now we can call the function from the app file.

  ```
  import Icons from "../helpers/icons";

  (...)

  export default class App extends Component {
  constructor(props) {
    super(props);

    Icons()
```

And the icons still work.