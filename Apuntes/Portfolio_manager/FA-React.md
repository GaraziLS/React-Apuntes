## Installing Font Awesome in React

We need to open the terminal because we need to install dependencies in order to make Font Awesome work in React. 

```
npm i @fortawesome/fontawesome-svg-core @fortawesome/free-solid-svg-icons @fortawesome/react-fontawesome
```

Then, in the **app.js** file, we're going to import this.

```
import { library } from "@fortawesome/fontawesome-svg-core";
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome";
import { faTrash, faSignOutAlt } from "@fortawesome/free-solid-svg-icons";
```

Now we need to tell the app which icons we will pull in. Below all the imports, we'll call those:

```
library.add(faTrash, faSignOutAlt);
```

In the **portfolio-sidebar-list.js** file, we'll test this out by calling the component itself where the delete should be, and we will call the component and pass the icon name as a prop:

```
<a onClick={() => props.handleDeleteClick(PortfolioItem)}><FontAwesomeIcon icon= "trash"/></a>
```

Now we can add styles.

> For the delete styles it's recommended tio put an ease-in-and-ot transition and a red color.

## Font Awesome Naming Requirements for React Components

In the **navigation-container.js** we'll import Font Awesome:

```
import { FontAwesome } from "@fortawesome/react-awesome";
```

Now that we have that, we can call this component like before, but Font Awesome in React is quite tricky when multiple word icon names are involved. But in the import of the icons (in the **app.js** file) we have the names for the icons (after the fa portion: in this case, just ```SignOutAlt```, that must be written all in lowercase.)

```
<FontAwesomeIcon icon="sign-out-alt" />
```

Now we can add styles.