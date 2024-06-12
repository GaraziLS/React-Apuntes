## Installing React Dropzone Component and Performing a Security Audit Fix

Now we'll install a library called Dropzone React Component to allow drag and drop type content. If vulnerabilities are detected, we'll type ``npm audit fix`` or ``npm audit``.

## Integrating React Dropzone Component into the Portfolio Form

In the **portfolio-form.js** file, we'll import the Dropzone component, as well as some style sheets specific to it that allow image previews and have some icons. To do this we'll go to the node_modules directory and search the Dropzone component. Inside of it we'll go to dist > min and search for dropzone.min.css. We'll repeat the process for another library (react-dropzone-component).

```
import DropzoneComponent from "react-dropzone-component";

import "../../../node_modules/dropzone/dist/min/dropzone.min.css"
```

Once we have that, we'll add a couple of configuration methods. In the portfolio-form.js file, right above the FormData, we'll type this (all of this is in the documentation)

```
componentConfig() {
    return {
      iconFiletypes: [".jpg", ".png"], 
      showFiletypeIcon: true,
      postUrl: "https://httpbin.org/post"
    }
  }

```

DropzoneComponent usually is built for people who want to build in a feature where you upload the image and then the image automatically goes and it hits an API and it starts uploading.

We don't want that, we want to be able to upload the file and then keep working on the form and then only when we hit Submit do we want the file passed to the API. So what we're doing here is we're going to pass in a mock URL, it will be mocked, which means it's going to almost kind of like pretend to hit this URL.

> The mock url recieves an API verb and always returns true.

Now we're goint to add another configuration.

```
djsConfig() {
    return {
      addRemoveLinks: true,
      maxFiles: 1
    }
  }

```

And bind these two as well. Now, below the textarea section we'll add a div to call the Dropzone Component and pass it props:

```
<div className="image-uploaders">
   <DropzoneComponent
     config={this.componentConfig}
     djsConfig={this.djsConfig}
     ></DropzoneComponent>
 </div>
```

This will generate an error of ```No url provided`` and also ``Can't perform a React state update on an unmounted component.`` To fix this we have to call the new props as regular methods.

```
<div className="image-uploaders">
    <DropzoneComponent
      config={this.componentConfig()}
      djsConfig={this.djsConfig()}
      ></DropzoneComponent>
  </div>
```

> We know we passed in the URL but what happened is because we hadn't actually invoked the function, this had not been returned yet.

## Uploading Thumbnail Images to the API in React

 Once the image is added to the dropzone component here we're gonna be able to push it up through the API to the server and then have it render here on the screen.

 In the **portfolio-form.js** file, we need to build out a method for handling the process for when an image is dropped onto that dropzone component. We're going to add this above the dropzone configurations and below the binds. All of the processes inside this function are in the dropzone component documentation. Then, we're going to bind the function as well:

```
  handleThumbDrop() {
    return {
      addedfile: file => this.setState({ thumb_image: file })
    };
  }
 ```

 So we have this drop process which we are going to pass to the dropzone component, and whenever something, specifically a file, is dropped on the component then it is going to check and it's gonna look inside of this return statement and it's gonna see what files or what methods do I have access to.

It's gonna find this addedfile function here, and then it is gonna run it. It's gonna pass in the file that was dropped in, and then it's going to say, okay, I want to setState with that file. 

Now we have to call it inside the Dropzone component itself. **NOTE: THIS HAS TO BE CALLED EXACTLY LIKE THIS:**

```
<div className="image-uploaders">
   <DropzoneComponent
     config={this.componentConfig()}
     djsConfig={this.djsConfig()}
     eventHandlers={this.handleThumbDrop()}
     ></DropzoneComponent>
 </div>

```

Now we'll update the BuildForm method because it's the piece that connects the state with the API. It's important to note that if we do this with the same structure as the rest of the append objects, the function is going to expect an actual file and when it finds nothing is going to throw an error. We want to append this item if it exists. If it was text, however, we could have sent null values.

```
buildForm() {
    let formData = new FormData();

    formData.append("portfolio_item[name]", this.state.name);
    formData.append("portfolio_item[description]", this.state.description);
    formData.append("portfolio_item[url]", this.state.url);
    formData.append("portfolio_item[category]", this.state.category);
    formData.append("portfolio_item[position]", this.state.position);


    if (this.state.thumb_image) {
      formData.append("portfolio_item[thumb_image]", this.state.thumb_image);
    }
    

    return formData;
  }
  ```

  ## Adding the Banner and Logo Image Uploaders

  We'll now add the other two image types. In the **portfolio-form.js file**, we're going to create a few more handlers, and bind them as well.

  ```
  handleBannerDrop() {
    return {
      addedfile: file => this.setState({ banner_image: file })
    };
  }
  
  handleLogoDrop() {
    return {
      addedfile: file => this.setState({ logo: file })
    };
  }
  ```

  We need three handlers because each one of those has a different state. Using the same handler would cause issues.

  Inside the *image-uploaders* class we'll call the Dropzone component once again to add the other event handlers:

  ```
  <div className="image-uploaders">
         <DropzoneComponent
           config={this.componentConfig()}
           djsConfig={this.djsConfig()}
           eventHandlers={this.handleThumbDrop()}
           ></DropzoneCompo
         <DropzoneComponent
           config={this.componentConfig()}
           djsConfig={this.djsConfig()}
           eventHandlers={this.handleBannerDrop()}
         <DropzoneComponent
           config={this.componentConfig()}
           djsConfig={this.djsConfig()}
           eventHandlers={this.handleLogoDrop()}
     />
       </div>
```

We also need to update the consitional appenders to allow the other image types to upload as well:

```
if (this.state.thumb_image) {
      formData.append("portfolio_item[thumb_image]", this.state.thumb_image);
    }

    if (this.state.banner_image) {
      formData.append("portfolio_item[banner_image]", this.state.banner_image);
    }

    if (this.state.logo) {
      formData.append("portfolio_item[logo]", this.state.logo);
    }
```

## Overview of React Refs to Clear the Form and Image Uploaders

As you may have noticed, whenever we submit a form on our portfolio, nothing changes on the form so we're able to add the callback so that we could update the sidebar list.

That is great but if we want to be able to have the traditional kind of form behavior, what a user expects is to be able to submit a form and then all of the elements here should be removed, they should just automatically be cleared off as soon as the form was successfully submitted so that the user could enter in another record.

And so that's what we're going to do and we are gonna learn a new concept in this guide. And this is the concept of refs, which is short for references.

Now, this is also going to give us the opportunity to talk about how React works with the DOM, because is a virtual DOM... normally. React doesn't actually allow us directly to interact with the real elements on the page. Instead, it takes up all that JSX code and it converts it into HTML for us, and then it performs the selection. However, there ARE times in which you need to interact with the real DOM, and here come the **Refs**, short for **References**.

It's discouraged to overuse refs because they can hurt performance and cause bugs. (Note: as of 2024, these are old). However, to interact with libraries using refs is recommended.

In the **portfolio-form.js** file, we'll define a ref inside the constructor and below the binds.

```
this.handleLogoDrop = this.handleLogoDrop.bind(this);
  
  this.thumbRef = React.createRef();
  this.bannerRef = React.createRef();
  this.logoRef = React.createRef();
    
  }
```

Now we'll pass these to the Dropzone component.

```
<divlassName="image-uploaders">
       <DropzoneComponent
         ref={this.thumbRef}
         config={this.componentConfig()}
         djsConfig={this.djsConfig()}
         eventHandlers={this.handleThumbDrop()}
         ></DropzoneCompone
       <DropzoneComponent
         ref={this.bannerRef}
         config={this.componentConfig()}
         djsConfig={this.djsConfig()}
         eventHandlers={this.handleBannerDrop()}
  
       <DropzoneComponent
         ref={this.logoRef}
         config={this.componentConfig()}
         djsConfig={this.djsConfig()}
         eventHandlers={this.handleLogoDrop()}
   />
  ```

   Now these references are now tied directly into the DropzoneComponent. This is not something that has anything to do with the component library, this is something in React where React gives us the ability to place a handle directly inside of one of these components on any component on the page. If you wanted to, you could put a reference in every single div on the page and then you could access it just like a regular HTML element but that kinda kills the whole point of using React.

   Now, inside of the **handleSubmit** method, we'll store those references inside an array just below the .then portion but inside it, and will call a forEach loop to loop over that ref.

   ```
   handleSubmit(event) {
    axios
      .post(
        "https://garazils.devcamp.space/portfolio/portfolio_items",
        this.buildForm(),
        { withCredentials: true }
      )
      .then(response => {
        this.props.handleSuccessfulFormSubmission
        console.log("response", response);

        [this.thumbRef, this.bannerRef, this.logoRef].forEach(ref => {
          ref.current.dropzone.removeAllFiles();
        });
      })

      .catch(error => {
        console.log("portfolio form handleSubmit error", error);
      });

    event.preventDefault();
  }
  ```

  Now we'll add the state to make the program clear it, so now we get this:

  ```
  handleSubmit(event) {
    axios
      .post(
        "https://garazils.devcamp.space/portfolio/portfolio_items",
        this.buildForm(),
        { withCredentials: true }
      )
      .then(response => {
        this.props.handleSuccessfulFormSubmission
        console.log("response", response);

        this.setState({
          name: "",
          description: "",
          category: "Bussiness",
          position: "",
          url: "",
          thumb_image: "",
          banner_image: "",
          logo: ""
        });

        [this.thumbRef, this.bannerRef, this.logoRef].forEach(ref => {
          ref.current.dropzone.removeAllFiles();
        });
      })

      .catch(error => {
        console.log("portfolio form handleSubmit error", error);
      });

    event.preventDefault();
  }
  ```

  And the form is cleared.

  ## Creating a Form Input Mixin

  We're going to add some styles to the form now. In the **portfolio-form.js** file, we're gona style the form itself, so we can remove the h1 and the div tag inside the render function. Inside the form tag we're going to add a class name.

  Now we're going to create a dedicated scss file and in the mixins one we're going to add a few as well.

  > Using mixins is a good idea when you want to give the same style to elements, such as buttons.


## How to Implement Custom Select and Textarea Styles and grid styles

> Styles aplied to textareas that don't have a class name are applied globally to every textarea. If you want to override mixin styles or whatever, be sure to put those files below the mixins.

## Updating the Dropzone Labels with Child Components to Finalize the Form

We'll now update the dropzone labels to put custom messages there. In the **portfolio-form.js** file, the image uploaders have two classes. Image uploaders and three-columns. After deleting the three-column class (which was used in the *grid.scss* file), we'll now open the **portfolio-form.scss** styles and copy the other class there. Now we'll open the **mixins.scss** file and create a base grid:

```
@mixin base-grid() {
  display: grid;
  grid-gap: 21px;
}
```

Now we'll include that inside the image-uploaders and in the grid.scss file to shrink code.

Once we have that, we'll style the button (we already had the mixin, we only have to add the class).

Now we can update the dropzone labels. In the console we'll look at the data React generates until we find the dropzone component. It should have a class for the message, ```dz-default dz-message```. Now we can use that class, pass it to Dropzoe components, and update the messages:

```
<div className="image-uploaders three-column">
  <DropzoneComponent
    ref={this.thumbRef}
    config={this.componentConfig()}
    djsConfig={this.djsConfig()}
    eventHandlers={this.handleThumbDrop()}
      <div className="dz-message">Drop a Thumbnail image</div>
    </DropzoneComponent>
```

Repeat the process for the other two. 

> In reality, we just slided child components.