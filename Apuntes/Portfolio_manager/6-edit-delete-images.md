## Guide to Multi Line Ternary Operators in React for Showing and Hiding the Dropzone Components

Now that we have the basic functionality for our update form working perfectly, it's time to move on to how we can work with images, so the dropzone will be populated with images if they exist.

In the **portfolio-form.js** component, inside of the ComponentDidUpdate() method and inside the state, we'll call in new keys and will populate them with images if they exist.

``
componentDidUpdate() {
    if (Object.keys(this.props.portfolioToEdit).length > 0) {
      const {
        id,
        name,
        description,
        category,
        position,
        url,
        thumb_image_url,
        banner_image_url,
        logo_url,
      } = this.props.portfolioToEdit;

      this.props.clearPortfolioToEdit();

      this.setState( {
        id: id,
        name: name || "",
        description: description || "",
        category: category || "eCommerce",
        position: position || "",
        url: url || "",
        editMode: true,
        apiUrl: `https://garazils.devcamp.space/portfolio/portfolio_items/${id}`,
        apiAction: "patch",
        thumb_image: thumb_image_url || "",
        banner_image: banner_image_url || "",
        logo: logo_url || ""
      });
    }
    ``

> The **image_tumb** portion already exists in the initial state. We'll repeat the process with the logo and the banner images.

Inside the same file (portfolio-form.js) we'll go to the first dropzone component. We'll use a ternary operator there.

```
<div className="image-uploaders three-column">
   {this.state.thumb_image && this.state.editMode ? (
   
     <img src={this.state.thumb_image}/>
   ) : (
   <DropzoneComponent
   ref={this.thumbRef}
   config={this.componentConfig()}
   djsConfig={this.djsConfig()}
   eventHandlers={this.handleThumbDrop()}
   >
     <div className="dz-message">Drop a Thumbnail image</div>
   </DropzoneComponent>)}

```

Now we'll repeat that with the other dopzone components, as well as add style, because the images don't fit inside the boxes right now.

So we'll wrap the image inside that ternary operator inside a div:

```
<div className="image-uploaders three-column">
    {this.state.thumb_image && this.state.editMode ? (
      <div className="portfolio-manager-image-wrapper">
        <img src={this.state.thumb_image}/>
      </div>
    ) : (
    <DropzoneComponent
    ref={this.thumbRef}
    config={this.componentConfig()}
    djsConfig={this.djsConfig()}
    eventHandlers={this.handleThumbDrop()}
    >
      <div className="dz-message">Drop a Thumbnail image</div>
    </DropzoneComponent>)}
```

And after repeating that with the other dropzones, we'll have this:

```
<div className="image-uploaders three-column">
                {this.state.thumb_image && this.state.editMode ? (
                  <div className="portfolio-manager-image-wrapper">
                    <img src={this.state.thumb_image}/>
                  </div>
                ) : (
                <DropzoneComponent
                ref={this.thumbRef}
                config={this.componentConfig()}
                djsConfig={this.djsConfig()}
                eventHandlers={this.handleThumbDrop()}
                >
                  <div className="dz-message">Drop a Thumbnail image</div>
                </DropzoneComponent>)}


                <DropzoneComponent
                  ref={this.thumbRef}
                  config={this.componentConfig()}
                  djsConfig={this.djsConfig()}
                  eventHandlers={this.handleThumbDrop()}
                  >

                    <div className="dz-message">Drop a Thumbnail image</div>
                  </DropzoneComponent>

                  {this.state.banner_image && this.state.editMode ? (
            <div className="portfolio-manager-image-wrapper">
              <img src={this.state.banner_image} />
            </div>
          ) : (
            <DropzoneComponent
              ref={this.bannerRef}
              config={this.componentConfig()}
              djsConfig={this.djsConfig()}
              eventHandlers={this.handleBannerDrop()}
            >
              <div className="dz-message">Banner</div>
            </DropzoneComponent>
          )}

          {this.state.logo && this.state.editMode ? (
            <div className="portfolio-manager-image-wrapper">
              <img src={this.state.logo} />
            </div>
          ) : (
            <DropzoneComponent
              ref={this.logoRef}
              config={this.componentConfig()}
              djsConfig={this.djsConfig()}
              eventHandlers={this.handleLogoDrop()}
            >
              <div className="dz-message">Logo</div>
            </DropzoneComponent>
          )}
        </div>
```

Now we'll add styles inside the portfolio-form.scss.

## Adding Delete Image Links to the Image Thumbnails

Now we'll add delete links to the images, to remove them if they're no longer needed.

In the portfolio-form.js file, we'll create a new function called ```deleteImage``.

```
deleteImage(image) {
    console.log("deleteImage", imageType)
  }
```
> The console log is temporary, just to make sure everything works.

Now inside the ternary operators we'll create a new class and type this code (change the string of the deleteImage param to match the code portion):

```
<img src={this.state.thumb_image} />
                <div className="image-removal-link">
                  <a onClick={() => this.deleteImage("thumb_image")}>
                  Remove file
                </a>
              </div>
```

Now we'll add styles.

### Fixing a bug

Before continuing, there's a bug in the app. If only some of the three dropzones are filled, when filling the others there's a bug related to images not showing, and no longer is a dropzone component. The system is updating the state and is showing an image that doesn't exist. To fix this we're going to give different names to the images when they're in edit mode.

So with the edits, we have: 

```
componentDidUpdate() {
    if (Object.keys(this.props.portfolioToEdit).length > 0) {
      const {
        id,
        name,
        description,
        category,
        position,
        url,
        thumb_image_url,
        banner_image_url,
        logo_url
      } = this.props.portfolioToEdit;

    this.props.clearPortfolioToEdit();

      this.setState({
        id: id,
        name: name || "",
        description: description || "",
        category: category || "eCommerce",
        position: position || "",
        url: url || "",
        editMode: true,
        apiUrl: `https://jordan.devcamp.space/portfolio/portfolio_items/${id}`,
        apiAction: "patch",
        thumb_image_url: thumb_image_url || "",
        banner_image_url: banner_image_url || "",
        logo_url: logo_url || ""
      });
      ```
```
      <div className="image-uploaders">
          {this.state.thumb_image_url && this.state.editMode ? (
                <div className="portfolio-manager-image-wrapper">
              <img src={this.state.thumb_image_url} /
              <div className="image-removal-link">
                <a onClick={() => this.deleteImage("thumb_image")}>
                  Remove file
                </a>
              </div>
            </div>
          ) : (
                <DropzoneComponent
              ref={this.thumbRef}
              config={this.componentConfig()}
              djsConfig={this.djsConfig()}
              eventHandlers={this.handleThumbDrop()}
            >
              <div className="dz-message">Thumbnail</div>
            </DropzoneComponent>
          )
          {this.state.banner_image_url && this.state.editMode ? (
                <div className="portfolio-manager-image-wrapper">
              <img src={this.state.banner_image_url} /
              <div className="image-removal-link">
                <a onClick={() => this.deleteImage("banner_image")}>
                  Remove file
                </a>
              </div>
            </div>
          ) : (
                <DropzoneComponent
              ref={this.bannerRef}
              config={this.componentConfig()}
              djsConfig={this.djsConfig()}
              eventHandlers={this.handleBannerDrop()}
            >
              <div className="dz-message">Banner</div>
            </DropzoneComponent>
          )
          {this.state.logo_url && this.state.editMode ? (
                <div className="portfolio-manager-image-wrapper">
              <img src={this.state.logo_url} /
              <div className="image-removal-link">
                <a onClick={() => this.deleteImage("logo")}>Remove file</a>
              </div>
            </div>
          ) : (
                <DropzoneComponent
              ref={this.logoRef}
              config={this.componentConfig()}
              djsConfig={this.djsConfig()}
              eventHandlers={this.handleLogoDrop()}
            >
              <div className="dz-message">Logo</div>
            </DropzoneComponent>
          )}
          ```

Now the error should be fixed. 

## Deleting images

Now we are ready to delete images. We want to click edit and if there's an image we want to contact the API to tell that this image should be deleted. We need the id to indicate the system which image will be deleted. Inside the code, we don't have the id if it's a new record, but we do if we're in edit mode.

In the **portfolio-form.js** file, inside the deleteImage method, we're going to call axios and pass the corresponding url along with custom params (with the id and the image type interpolated).

```
deleteImage(imageType) {
  axios.delete(
    `https://api.devcamp.space/portfolio/delete-portfolio-image/${this.state
      .id}?image_type=${imageType}`,
    { withCredentials: true }
  );
}
```

Then, we'll build the promise.

```
deleteImage(imageType) {
  axios.delete(
    `https://api.devcamp.space/portfolio/delete-portfolio-image/${this.state
      .id}?image_type=${imageType}`,
    { withCredentials: true }
  ).then(response => {
    console.log("deleteImage", response);
  }).catch(error => {
    console.log("deleteImage error", error);
  });
}
```

Now we'll update the state. Inside that promise we'll set state, but we'll do it dynamically. Whenever we pass dynamic objects we use brackets and type this:

```
deleteImage(imageType) {
  axios.delete(
    `https://api.devcamp.space/portfolio/delete-portfolio-image/${this.state
      .id}?image_type=${imageType}`,
    { withCredentials: true }
  ).then(response => {
    this.setState({
      [`${imageType}_url`]: ""
    })
```

Now this will delete the image, and the conditional section will be automatically false because the state is empty, and it's going to show the dropzone component:

```
{this.state.thumb_image_url && this.state.editMode ? (
            <div className="portfolio-manager-image-wrapper">
              <img src={this.state.thumb_image_url} />

              <div className="image-removal-link">
                <a onClick={() => this.deleteImage("thumb_image")}>
                  Remove file
                </a>
              </div>
            </div>
          ) : (
            <DropzoneComponent
              ref={this.thumbRef}
              config={this.componentConfig()}
              djsConfig={this.djsConfig()}
              eventHandlers={this.handleThumbDrop()}
            >
```

 The string nterpolation allows us to select the image type and then concats the _url portion.