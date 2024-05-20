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

> Remember to add an active class to links in order to create an effect when that link is clicked.

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