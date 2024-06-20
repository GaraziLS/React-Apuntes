## Configuring Dropzone for Blog Featured Image

Up until this point, if we wanted to upload a featured image for our blog, we had to go through DevCamp Space. So in this guide, we're gonna start the process of adding our dropzone image component so that we can start adding images directly to our blog.

In the **blog-form.js** file we'll import dropzone, and then we're going to bind and create a few functions (these are in the docs).

```
import DropzoneComponent from "react-dropzone-component"

(...)

this.componentConfig = this.componentConfig.bind(this);
 this.djsConfig = this.djsConfig.bind(this);
 this.handleFeaturedImageDrop = this.handleFeaturedImageDrop.bind(this);

(...)

componentConfig() {
  return {
    iconFiletypes: [".jpg", ".png"]
    showFiletypeIcon: true,
    postUrl: "https://httpbin.org/post"
  }
}

djsConfig() {
    return {
      addRemoveLinks: true,
      maxFiles: 1
    }
  }
```

**Review Time**

DropzoneComponent usually is built for people who want to build in a feature where you upload the image and then the image automatically goes and it hits an API and it starts uploading.

We don't want that, we want to be able to upload the file and then keep working on the form and then only when we hit Submit do we want the file passed to the API. So what we're doing here is we're going to pass in a mock URL, it will be mocked, which means it's going to almost kind of like pretend to hit this URL.

> The mock url recieves an API verb and always returns true.

**End of Review**

Now we're going to talk about behavior, what happens when a user drops a file or picks one out from the file system. So let's take our handleFeaturedImageDrop function.

```
import DropzoneComponent from "react-dropzone-component"

(...)

this.componentConfig = this.componentConfig.bind(this);
 this.djsConfig = this.djsConfig.bind(this);
 this.handleFeaturedImageDrop = this.handleFeaturedImageDrop.bind(this);

(...)

componentConfig() {
  return {
    iconFiletypes: [".jpg", ".png"]
    showFiletypeIcon: true,
    postUrl: "https://httpbin.org/post"
  }
}

djsConfig() {
    return {
      addRemoveLinks: true,
      maxFiles: 1
    }
  }

handleFeaturedImageDrop() {
    return {
      addedfile: file => this.setState({featured_image: file})
    }
  }

```

We'll add the featured_image key to the state as well.

Now we're going to call the component below the rich text editor.

```
<div className="image-uploaders">
  <DropzoneComponent
    config={this.componentConfig()}
    djsConfig={this.djsConfig()}
    eventHandlers={this.handleFeaturedImageDrop()}
  ></DropzoneComponent>
</div>
```

> If you want the function to run immediately you add (), if not, not.

Now we're going to add a class with a custom label.

```
<div className="image-uploaders">
  <DropzoneComponent
    config={this.componentConfig()}
    djsConfig={this.djsConfig()}
    eventHandlers={this.handleFeaturedImageDrop()}
    <div className="dz-message">Drop a Thumbnail image</div>
  ></DropzoneComponent>
</div>
```

Now the dropzone is working, but we can't update images yet.

## Adding the Ability to Upload Featured Images to Blogs

Now that we've added the DropzoneComponent to our blog-form, we only have a few small changes that we need to make in order to update our entire form so that it sends up that media object, that image object, in a way the API can understand it.

In the **blog-form.js** file, we need to make sure and check to see if we have a featured image uploaded or not, so we're gonna add a check here.

```
buildForm() {
    let formData = new FormData();

    formData.append("portfolio_blog[title]", this.state.title);
    formData.append("portfolio_blog[blog_status]", this.state.blog_status);
    formData.append("portfolio_blog[content]", this.state.content);

    if (this.state.featured_image) {
      formData.append(
        "portfolio_blog[featured_image]", 
        this.state.featured_image
      );
    }

    return formData;
  }
```

Now we can upload images, but we want to clear the image itself. We'll use the **portfolio-form.js** file as reference. In that file we used refs, which allow to reach in and work with that component like it is our own, like it is a piece of HTML, as they allow to interact with the DOM.
 
 We'll first bind the function, and then call this inside the promise.

 ```
this.componentConfig = this.componentConfig.bind(this);
    this.djsConfig = this.djsConfig.bind(this);
    this.handleFeaturedImageDrop = this.handleFeaturedImageDrop.bind(this);

    this.featuredImageRef = React.createRef();

(...)

handleSubmit(event) {
    axios
      .post(
        "https://garazils.devcamp.space/portfolio/portfolio_blogs",
        this.buildForm(),
        { withCredentials: true }
      )
      .then(response => {
        if (this.state.featured_image) {
          this.featuredImageRef.current.dropzone.removeAllFiles();
        }

 ```

 Now we'll add the state to make the program clear it, so now we get this:

 ```
 handleSubmit(event) {
    axios
      .post(
        "https://garazils.devcamp.space/portfolio/portfolio_blogs",
        this.buildForm(),
        { withCredentials: true }
      )
      .then(response => {
        if (this.state.featured_image) {
          this.featuredImageRef.current.dropzone.removeAllFiles();
        }

        this.setState({
          title: "",
          blog_status: "",
          content: "",
          featured_image: ""
        });
```

Now we will pass the ref as a prop inside the component:

```
<DropzoneComponent
     ref={this.featuredImageRef}
```

Now we'll add styles.