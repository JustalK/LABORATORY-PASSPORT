# LABORATORY-PASSPORT

I discover few weeks ago the library `Passport.js` that you can find at this URL : http://www.passportjs.org/

It makes multiple Authentication through google, facebook, twitter and so on easy using just the ClientId and ClientSecret of the different platform.

It becomes a must have in my toolbox for managing this kind of challenge.

## Plan

1. [How to use Passport.js](How-to-use-Passport.js)
2. [How to create clientID and clientSecret for facebook](How-to-create-clientID-and-clientSecret-for-facebook)
3. [How to create clientID and clientSecret for google](How-to-create-clientID-and-clientSecret-for-google)

## How to use Passport.js

1. Install Passport.js

```
$ npm install Passport.js
```

In the Express server, use :

```
const passport = require('passport');

app.use(passport.initialize());
app.use(passport.session());

passport.serializeUser(function (user, cb) {
  cb(null, user);
});

passport.deserializeUser(function (obj, cb) {
  cb(null, obj);
});
```

2. Install the dependencies depending of the passport we need

**facebook**

```
$ npm install passport-facebook
```

**google**

```
$ npm install passport-google-oauth
```

3. Enable the Passport depending of the passport

**facebook**

```
const FacebookStrategy = require('passport-facebook').Strategy;

passport.use(new FacebookStrategy({
    clientID: config.facebookAuth.clientID,
    clientSecret: config.facebookAuth.clientSecret,
    callbackURL: config.facebookAuth.callbackURL
  }, function (accessToken, refreshToken, profile, done) {
    return done(null, profile);
  }
));
```

**google**

```
const GoogleStrategy = require('passport-google-oauth').OAuth2Strategy;

passport.use(new GoogleStrategy({
    clientID: config.googleAuth.clientID,
    clientSecret: config.googleAuth.clientSecret,
    callbackURL: config.googleAuth.callbackURL
  }, function (accessToken, refreshToken, profile, done) {
    return done(null, profile);
  }
));
```

4. Add the ClientID and ClientSecret inside the config.js (see below how to get them)

5. Create the route for getting the information out of the Authentication

The scope depend of the Strategy (facebook or google...) and can be find on the documentation of the strategy (google documentation or facebook documentation...)

**facebook**

```
router.get('/profile', isLoggedIn, function (req, res) {
  console.log(req.user)
});

router.get('/auth/facebook', passport.authenticate('facebook', {
  scope: ['public_profile', 'email']
}));

router.get('/auth/facebook/callback',
  passport.authenticate('facebook', {
    successRedirect: '/profile',
    failureRedirect: '/error'
  })
);
```

**google**

```
router.get('/profile_google', isLoggedIn, function (req, res) {
  console.log(req.user)
});

router.get('/auth/google', passport.authenticate('google', {
  scope: ['profile', 'email']
}));

router.get('/auth/google/callback',
  passport.authenticate('google', {
    successRedirect: '/profile_google',
    failureRedirect: '/error'
  })
);
```


## How to create clientID and clientSecret for facebook

1. First, connect to the facebook developer console : https://developers.facebook.com/

![Alt text](documentations/facebook/1.png?raw=true "Facebook")

2. Click on `create a new app` and choose the type of app (`none` in my case)

![Alt text](documentations/facebook/2.png?raw=true "Facebook")

3. Add the name to display in the facebook developer interface

![Alt text](documentations/facebook/3.png?raw=true "Facebook")

4. Click on facebook login

![Alt text](documentations/facebook/4.png?raw=true "Facebook")

5. Click on `www` since we will be building a website

![Alt text](documentations/facebook/5.png?raw=true "Facebook")

6. Since we will be testing in it locally, we will enter the website : `http://localhost:3000/`

![Alt text](documentations/facebook/6.png?raw=true "Facebook")

7. We then arrive on a page where we can find the ClientId (App ID) and the ClientSecret (App Secret) to enter in our `config.js` file

![Alt text](documentations/facebook/7.png?raw=true "Facebook")

## How to create clientID and clientSecret for google

1. First, connect to the google console : https://console.cloud.google.com/

![Alt text](documentations/google/1.png?raw=true "Google")

2. Search in the bar on the top `oauth` and click on `identifiants`

![Alt text](documentations/google/2.png?raw=true "Google")

3. Once the page loaded, click on the top `create identifiants`

![Alt text](documentations/google/3.png?raw=true "Google")

4. In the dropdown, click on `ID Client OAuth`

![Alt text](documentations/google/4.png?raw=true "Google")

5. Choose the type of application (`web application` in this case), add a name and dont forget to add the redirection URI at the bottom. Since I am working locally, it will be : http://localhost:3000

![Alt text](documentations/google/5.png?raw=true "Google")

6. You then will get a popup with the ClientID and ClientSecret that you can copy and paste into the `config.js` file.

![Alt text](documentations/google/6.png?raw=true "Google")
