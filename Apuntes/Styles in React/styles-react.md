 ## Create a Scss file to import and organize styles in React

 Go to the src > style folder first. There's gonna be an scss file.  We want this file to be the entrypoint, which means that all the other files will be imported from here. Create a file inside this folder and now export it to main.scss.

 ```
 @import "./base.scss"
 ```

**IMPORTS WORK DIFFERENTLY IN SCSS**

 > You can organize this however you want, there are many ways to do it.

## Custom fonts in React

Bring fonts from google fonts into the index.html file (that file is in the static directory) and then copy the code into the scss file. Remember to remove everything from the base scss file to avoid issues.

## Fexbox in React

We're gonna create a scss file called navigation. The classes required to make the styles work will go into the navigation-container.js file to write the classes. It's important to note that all of this will go in a single class. Now, the class word is a reserved one in react, so we can't say ```<div class=""></div>``` as usual. Instead, we'll put a **className**.

> Remember to add an active class to navlinks and right after the route in order to create an effect when that link is clicked.

```
render() {
        return (
        <div className='nav-wrapper'>
            <div className='left-side'>
                <div className='navlink-wrapper' >
                    <NavLink exact to="/" activeClassName="navlink-active"> Home</NavLink>
                </div>
                <div className='navlink-wrapper'>
                    <NavLink exact to="/about-me" activeClassName="navlink-active">About</NavLink>
                </div>
                <div className='navlink-wrapper'>
                    <NavLink exact to="/contact" activeClassName="navlink-active">Contact</NavLink>
                </div>
                <div className='navlink-wrapper'>
                    <NavLink exact to="/blog" activeClassName="navlink-active">Blog</NavLink>
                </div>
            </div>
            <div className='right-side'>TEST</div>
        </div>
    
        
        );
    }
```

> React doesn't have < a > tags, but the chrome console transforms react to regular HTML and thus you can select those tags as usual.

## Variables in scss

> Instead of typing hexadecimal colors, you can rename them using ````$name```.

## Inline styles in react to render background images

We're in the portfolio-item.js file and:

> Divs can also be self closing: ```<div className="portfolio-img-background" style={{}}/>```

> The style tag has 2 sets of curly brackets because we're going to pass JS code, and inside of it there's an object.

> Inline styles must be written in CamelCase like this: ```style={{backgroundImage: }}>```.

> Inside of that style we need to pass the item component like this: ```"url(" + thumb_image_url + ")"```

Now we just have to add styles.

## Overlaying text and images on background images in React

In the portfolio-item.js file, we'll write this:

```
<div className="img-text-wrapper">
          <div className="logo-wrapper">
            <img src={logo_url}/>
          </div>

        <div className="subtitle">{description}</div>
        </div>
    </div>
```

And then add styles.

> To use *position: absolute*, the parent must have *position: relative*.

```
.img-text-wrapper {
      position: absolute;
      top: 0;
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      height: 100%;
      text-align: center;
      padding-left: 100px;
      padding-right: 100px;

      .subtitle {
        transition: 1s ease-in-out;
        color: transparent;
      }
    }

    .img-text-wrapper:hover .subtitle {
      color: $teal;
      font-weight: 400;
    }

    .logo-wrapper img {
      width: 50%;
      margin-bottom: 20px;
    }
  ```

## Updating styles by adding event listeners

In the *portfolio-item.js* file, we're going to convert the functional component into a class component to manipulate state. So from this:

```
export default function(props) {
  const { id, description, thumb_image_url, logo_url } = props.item;
  return (
    <div className="portfolio-item-wrapper">
      <div
        className="portfolio-img-background"
        style={{
            backgroundImage: "url(" + thumb_image_url + ")"
        }}
      />
  )}
```

to this:

```
export default class PortfolioItem extends Component {
  constructor(props) {
    super(props);

    this.state = {
      portfolioItemClass: ""
    };
  }}
```

Then we'll create functions which later on will change the state:

```
handleMouseEnter() {
    this.setState({ portfolioItemClass: "image-blur" });
  }

  handleMouseLeave() {
    this.setState({ portfolioItemClass: "" });
  }
```

To make the functions work we'll add event listeners and pass in the functions:

```
return (
      <div
        className="portfolio-item-wrapper"
        onMouseEnter={() => this.handleMouseEnter()}
        onMouseLeave={() => this.handleMouseLeave()}
      >
)
```

And now we're going to add that class dynamically:


```
<div
          className={
            "portfolio-img-background " + this.state.portfolioItemClass
          }
```

## Using mixins to create button styles

In the *portfolio-container.js* component, we're gonna add a class to the buttons. Then we're going to create a mixins style page (remember that order of loading is scss is important, so put that file below the variables)

> In scss, when using actions (pseudo-classes?) you need to use the ampersand and then the colon: ```&:pseudo-class```.

## Add static images in React

The images must be in the src > static directory > assets > images. This isn't required, it's just for organization purposes.

In the *Auth.js* file we're going to type this to pass the image in inside the string interpolation (after importing it as a component):

```
import loginImg from "../../../static/assets/images/Auth/login.jpg";

className='left-column'
  style={{
      backgroundImage: `url(${})`
  }}
```

> Dots are for jumping between directories, just get the right amount by trial and error.