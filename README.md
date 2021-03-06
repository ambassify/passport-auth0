# passport-auth0

[![Build Status](https://travis-ci.org/auth0/passport-auth0.svg?branch=master)](https://travis-ci.org/auth0/passport-auth0)

This is the auth0 authentication strategy for Passport.js.

## Passport.js

[Passport](http://passportjs.org/) is authentication middleware for Node.js. Passport can be unobtrusively dropped in to any Express-based web application.

## Installation

	npm install passport-auth0

## Configuration

Take your credentials from the [settings](https://app.auth0.com/#/settings) section in the dashboard and initialize the strategy as follows:

~~~js
var Auth0Strategy = require('passport-auth0'),
    passport = require('passport');

var strategy = new Auth0Strategy({
   domain:       'your-domain.auth0.com',
   clientID:     'your-client-id',
   clientSecret: 'your-client-secret',
   callbackURL:  '/callback'
  },
  function(accessToken, refreshToken, extraParams, profile, done) {
    // accessToken is the token to call Auth0 API (not needed in the most cases)
    // extraParams.id_token has the JSON Web Token
    // profile has all the information from the user
    return done(null, profile);
  }
);

passport.use(strategy);
~~~

### State parameter

The Auth0 Passport strategy enforces use of `state` parameter in OAuth 2.0 [authorization requests](https://tools.ietf.org/html/rfc6749#section-4.1.1) and requires session support in Express to be enabled.

If you require the `state` parameter to be omitted (which is not recommended), you can suppress it when calling the Auth0 Passport strategy constructor:

~~~js
var Auth0Strategy = require('passport-auth0');

var strategy = new Auth0Strategy({
     domain: 'your-domain.auth0.com',
     // ...
     state: false
  },
  function(accessToken, refreshToken, extraParams, profile, done) {
    // ...
  }
);
~~~

## Usage

~~~js
app.get('/callback',
  passport.authenticate('auth0', { failureRedirect: '/login' }),
  function(req, res) {
    if (!req.user) {
      throw new Error('user null');
    }
    res.redirect("/");
  }
);

app.get('/login',
  passport.authenticate('auth0', {}), function (req, res) {
  res.redirect("/");
});
~~~

This way when you go to ```/login``` you will get redirected to auth0, to a page where you can select the identity provider.

If you want to force an identity provider you can use:

~~~javascript
app.get('/login/google',
  passport.authenticate('auth0', {connection: 'google-oauth2'}), function (req, res) {
  res.redirect("/");
});
~~~

If you want to specify an audience for the returned `access_token` you can:

~~~javascript
app.get('/login',
  passport.authenticate('auth0', {audience: 'urn:my-api'}), function (req, res) {
  res.redirect("/");
});
~~~

If you want to control the OIDC prompt you can use:

~~~javascript
app.get('/login',
  passport.authenticate('auth0', {prompt: 'none'}), function (req, res) {
  res.redirect("/");
});
~~~

## API access

If you want to get a list of connections or users from auth0, use the [auth0 module](https://github.com/auth0/node-auth0).

## Complete example

A complete example of using this library [here](http://github.com/auth0/passport-auth0).

## Documentation

For more information about [auth0](http://auth0.com) contact our [documentation page](http://docs.auth0.com/).

## Issue Reporting

If you have found a bug or if you have a feature request, please report them at this repository issues section. Please do not report security vulnerabilities on the public GitHub issue tracker. The [Responsible Disclosure Program](https://auth0.com/whitehat) details the procedure for disclosing security issues.

## Author

[Auth0](https://auth0.com/)

## License

This project is licensed under the MIT license. See the [LICENSE](LICENSE) file for more info.
