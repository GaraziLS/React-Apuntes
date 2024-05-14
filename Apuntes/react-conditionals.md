## React conditionals

Conditionals are implemented inside the render function. This allows, for example, to render a loading text while the API content is being rendered on the page. This way the screen isn't blank and the user won't exit the page.

```
render() {
        if(this.state.isLoading) {
            return <div>Loading...</div>
        }
        return (...)}
```

Now we have to put this *isLoading...* inside the initial state.

```
constructor() {
        super(); 
        this.state = {
            pageTitle: "Welcome to my portfolio",
             data: [
                {title: "First", category: "eCommerce"},
                 {title: "Second", category: "eCommerce"},
                 {title: "third", category: "Personal"},
                 {title: "fourth", category: "Hobby"}
                ],
            isLoading: true
            }};

```

Since the isLoading object is always true, the data can't be displayed. In a real-world app you would call an API, listen to when data comes back, and once it comes back the *this.state.isLoading* would turn to false, and since that conditional isn't needed anymore it would just jump to the next return statement.

## Ternary operators to implement conditionals in react

We now have several buttons on the navigation.js component. The idea is that only the site owner will be able to add blogs, while regular users won't. This is why we're going to use conditionals, or, in this case, a ternary operator, since it's the standard convention when writing JSX.

Remember that ternary operators are like this:

> Condition ? result if true : result if false

```
"Text" === "Text" ? "yay" : "nay" // yay
"Text" !=== "Text" ? "yay" : "nay" // nay
```

We don't have a way to check if the user signed up or not, for now we'll hardcode this directly on the add blog button.

```
{false ? <button>Add blog</button> : null}
```