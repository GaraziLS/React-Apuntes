 ## Create a Scss file to import and organize styles in React

 Go to the src > style folder first. There's gonna be an scss file.  We want this file to be the entrypoint, which means that all the other files will be imported from here. Create a file inside this folder and now export it to main.scss.

 ```
 @import "./base.scss"
 ```

**IMPORTS WORK DIFFERENTLY IN SCSS**

 > You can organize this however you want, there are many ways to do it.

## Custom fonts in React

Bring fonts from google fonts into the index.html file (that file is in the static directory) and then copy the code into the scss file. Remember to remove everything from the base scss file to avoid issues.
