# fast-food-friday

R&amp;D project combining AngularJS, Django Tastypie and Trigger.io 

Every Friday at [JPCreative](http://jpcreative.co.uk) we have a tradition of eating lunch together, sometimes forming a consensus on what to have proves difficult.  Until now... Introducing the Fast Fast Friday app.

It's a simple proof on concept app for iOS and Android that allows users to sign in via twitter, suggest and vote for their preferred lunch menu. It's coded completely in Python, HTML, CSS and JavaScript.  No native code is used at all.

The back end creates a API using [Django](https://www.djangoproject.com) and [Tastypie](http://tastypieapi.org). The front-end communicates to the API through [AngularJS](http://angularjs.org/) wrapped up into a native app by [trigger.io](https://trigger.io/)

## Installation

To package iOS apps with trigger.io you'll need OSX with Xcode. I'd also suggest you install Xcode command line tools.
To package Android apps will you need to install Java and the android SDK.

### Django

You'll need pip and virtualenvwrapper:

```bash
$ sudo easy_install pip
$ pip install virtualenvwrapper
```

Setup your environment and Django app:

```bash
$ mkvirtualenv authAppEnv
(authAppEnv)$ pip install -r requirements.txt
(authAppEnv)$ cd authApp
(authAppEnv)$ python manage.py syncdb
(authAppEnv)$ sudo python manage.py runserver YOUR-IP-HERE:80
```

The reason for using sudo and your IP with runserver will become clear when getting angular running locally.


Visit [https://dev.twitter.com/apps](https://dev.twitter.com/apps) to create a Twitter app and get a consumer and secret key. Set the Callback URL to http://YOUR-IP-HERE/logged-in.
In your browser go to: http://YOUR-IP-HERE/admin/socialaccount/socialapp/add/ and create a social app.
Sign out of Django admin, visit http://YOUR-IP-HERE/accounts/login/ and sign in with Twitter.
Hopefully that will prompt Twitter to authorize your account and redirect you to the login screen 'YAY'.
Please take a note of the URL query string as we'll need that for testing Angular.

### Angular.js

Open trigger/src/static/angular/services.js in your IDE of choice, and edit the line:

    var apiCall = $resource('http://YOUR-IP-HERE/api/v1/:type/:id',

Add your IP but don't add :80 or Angular will think it's another parameter and ignore the colon. 

We also need to update SiginInCtrl() with your IP, username, api_key and user id (saved from the query string in last step of installing Django) 

controllers.js:

    ...
    theURL = 'http://YOUR-IP-HERE/auth/?username=YOUR-TWITTER-USERNAME&api_key=YOUR-API-KEY&user=2';
    ...
    url: 'http://YOUR-IP-HERE/accounts/twitter/login/',
    pattern: 'http://YOUR-IP-HERE/auth/*'
    ...

To get the app running locally:

```bash
$ cd trigger/src
$ python -m SimpleHTTPServer
```

Visit [http://0.0.0.0:8000](http://0.0.0.0:8000/) in a web browser or iOS/Android simulator.  

### Trigger.io

Visit [trigger.io](https://trigger.io/) and follow the steps to get up and running. Create a new app and copy over the contents of trigger/src to the new trigger app src folder.  Edit the app config to include 'tabs'.

Run the app.