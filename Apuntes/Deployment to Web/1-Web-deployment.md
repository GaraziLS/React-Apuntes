## Deployment to Heroku

Now that we've done everything, it's time to get the entire portfolio live into the web. Github and Heroku accounts are required, as well as Heroku CLI. And we'll need to upload the code to github as well.

First we need to create a Heroku app. In the terminal, we'll type this:

```
heroku create AppName
```

The app name has to be unique.

Once the app is created, we must let the API know about our site. We'll grab the link that appeared in the console and will go to the share icon. Then we'll white list the url there.

Then you run the site by typing ```git push heroku master```. The terminal will give another link. Copy and paste and it's deployed.