Express.js middleware for OpenID Relying Party (aka OAuth 2.0 Client).

This module exposes two middlewares:

-  `.routes()`: install two routes one called `/login` and the other one `/callback`.
-  `.protect()`: is a middleware that redirects to `/login` if req.session.user is empty. This middleware preserves the url that the user tried to access in the session, so the callback can redirect back to it after a succesful login.

## Install

```
npm i express-openid-connect --save
```

## Requirements

Before installing the routes,

-  a body parser middleware for urlencoded content, eg: https://www.npmjs.com/package/body-parser
-  a session middleware like [express-session](https://www.npmjs.com/package/express-session) or [cookie-session](https://www.npmjs.com/package/cookie-session).
-  node v8 or greater
-  express v3 or greater

## Usage

```javascript
const auth = require('express-openid-client');

app.use(auth.routes({
  issuerBaseURL: `https://${process.env.AUTH0_DOMAIN}`,
  baseURL: 'https://myapplication.com',
  clientID: process.env.AUTH0_CLIENT_ID,
}))

app.use('/user', auth.protect(), (req, res) => {
  res.send(`hello ${req.session.user.name}`);
});

app.get('/', (req, res) => res.send("hello!"));
```

## Configuration through environment variables

Settings can be provided by environment variables as follows:

```
ISSUER_BASE_URL=https://my-domain.auth0.com
BASE_URL=https://myapplication.com
CLIENT_ID=xyz
```

then:

```javascript
const auth = require('express-openid-client');
app.use(auth.routes())
```

## Debugging

Start your application with the following environment variable to make this module output the debug logs.

```
DEBUG=express-openid-client:*
```

**WARNING:** this feature is intended only for development and must not be used in production since it will log sensitive information.

## License

This project is licensed under the MIT license. See the [LICENSE](LICENSE) file for more info.

