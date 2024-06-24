 ## Building About Page Styles for React Portfolio

 Now we're going to add styles to the about and contact pages.

 So we'll fix the about page for the div to cover the entire page.

 ```
 export default function() {
  return (
  <div className="content-page-wrapper">About</div>)
}
```

And we'll work from there.

After adding the grid columns and some text, we now need to add the image, and we can import it just like any other library.

```
import DemoPic from "../../../static/assets/images/auth/DemoPic.jpg"
```

Now we'll add the image with inline styles:

```
    <div
      className="left-column"
      style={{
        background: "url(" + DemoPic + ") no-repeat",
        backgroundSize: "cover",
        backgroundPosition: "center"
      }}
    />
```

> Go to styles-in-react.md file to learn more about this.

## Building Contact Page Styles for the React Portfolio