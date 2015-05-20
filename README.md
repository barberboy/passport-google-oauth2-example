Passport Google OAuth2 Example
==============================

This is an Express 4 application using Google for authentication via OAuth2.

Based on the [OAuth2 example](https://github.com/jaredhanson/passport-google-oauth/tree/master/examples/oauth2)
in Jared Hanson’s [passport-google-oauth](https://github.com/jaredhanson/passport-google-oauth), 
this Express 4 application uses Passport and the Passport Google OAuth strategy
to enable users to authenticate with their Google accounts.

The client id and client secret needed to authenticate with Google can be set up
from the [Google Developer's Console](https://console.developers.google.com).

## Install

1. `git clone https://github.com/barberboy/passport-google-oauth2-example myapp`
2. `cd myapp && npm install`
3.  Create a project and OAuth 2.0 client ID at <https://console.developers.google.com>
4.  Add clientID, clientSecret, and callbackURL to config/auth.json


#### Configure Strategy

The Google OAuth 2.0 authentication strategy authenticates users using a Google
account and OAuth 2.0 tokens.  The strategy requires a `verify` callback, which
accepts these credentials and calls `done` providing a user, as well as
`options` specifying a client ID, client secret, and callback URL.

```Javascript
var GoogleStrategy = require('passport-google-oauth').OAuth2Strategy;

passport.use(new GoogleStrategy({
    clientID: GOOGLE_CLIENT_ID,
    clientSecret: GOOGLE_CLIENT_SECRET,
    callbackURL: "http://127.0.0.1:3000/auth/google/callback"
  },
  function(accessToken, refreshToken, profile, done) {
    User.findOrCreate({ googleId: profile.id }, function (err, user) {
      return done(err, user);
    });
  }
));
```

#### Authenticate Requests

Use `passport.authenticate()`, specifying the `'google'` strategy, to
authenticate requests. Authentication with Google requires an extra scope
parameter.  For information, see the
[documentation](https://developers.google.com/+/api/oauth#scopes).

```Javascript
app.get('/auth/google',
  passport.authenticate('google', { scope: ['email profile'] }));

app.get('/auth/google/callback',
  passport.authenticate('google', { failureRedirect: '/login' }),
  function(req, res) {
    // Authenticated successfully
    res.redirect('/');
  });
```

## Credits

  - [Original example](https://github.com/jaredhanson/passport-google-oauth/tree/master/examples/oauth2)
    by [Jared Hanson](http://github.com/jaredhanson).

## License

[The MIT License](http://benbarber.mit-license.org/)