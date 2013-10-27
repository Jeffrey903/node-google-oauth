# google-oauth

Highly inspired by [github-oauth](https://github.com/maxogden/github-oauth)

Simple functions for doing oauth login with Google. Compatible with any node http server that uses handler callbacks that look like `function(req, res) {}`

## usage

```javascript
var googleOAuth = require('google-oauth')({
  googleClient: process.env['GOOGLE_CLIENT'],
  googleSecret: process.env['GOOGLE_SECRET'],
  baseURL: 'http://localhost',
  loginURI: '/login',
  callbackURI: '/callback',
  scope: 'user' // optional, default scope is set to user
})

require('http').createServer(function(req, res) {
  if (req.url.match(/login/)) return googleOAuth.login(req, res)
  if (req.url.match(/callback/)) return googleOAuth.callback(req, res)
}).listen(80)

googleOAuth.on('error', function(err) {
  console.error('there was a login error', err)
})

googleOAuth.on('token', function(token, serverResponse) {
  console.log('here is your shiny new google oauth token', token)
  serverResponse.end(JSON.stringify(token))
})

// now go to http://localhost/login
```

## bonus feature

```javascript
  googleOauth.addRoutes(myAppRouter)
  // where myAppRouter is a router that will work with:
  // myAppRouter.get('/google/login', function(req, res) {})
  // http://google.com/flatiron/director works like this

  // or even more bonus:
  googleOauth.addRoutes(myAppRouter, function(err, token, serverResponse, tokenResponse) {

  })

```
## license

BSD LICENSED