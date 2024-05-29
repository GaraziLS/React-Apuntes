## Installing React Dropzone Component and Performing a Security Audit Fix

Now we'll install a library called Dropzone React Component to allow drag and drop type content. If vulnerabilities are detected, we'll type ``npm audit fix`` or ``npm audit``.

## Integrating React Dropzone Component into the Portfolio Form

In the **portfolio-form.js** file, we'll import the Dropzone component, as well and some style sheets specific to it that allow image previews and have some icons. To di this we'll go to the node_modules directory and search de Dropzone component. Inside of it we'll go to dist > min and search for dropzone.min.css. We'll repeat the process for another library (react-dropzone-component).

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