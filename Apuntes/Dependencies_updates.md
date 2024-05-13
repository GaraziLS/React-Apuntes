# Dependencies update list

* package.json

> "sass": "^1.54.0", change to "node-sass": "4.13.1",

* Routing process
> type:  "react-router-dom": "4.2.2",

* The react-skeleton template webpack needs to be modified. In the webpack.config.js file, modify line 15:

> presets: ["react", "env", "stage-1"],

* Additionally, run the following command in your terminal in the project directory:

> npm install --save-dev babel-preset-stage-1

* To route without problems, type this in the package.json:

```
"react-dom": "^16.13.1",
"react-router-dom": "^4.2.2",
"webpack": "^4.20.2",
"webpack-cli": "^3.1.1",
```

