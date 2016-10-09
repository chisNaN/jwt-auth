#JWT Authentication

>Simple nodeJs Json Web Token Flow authentication

[Official introduction](https://jwt.io/introduction/)

:bulb: Easier to forge your requests with [Postman](https://www.getpostman.com/) than cURL

index.js

```js
const jwt = require('jsonwebtoken');
const expressJwt = require('express-jwt');
const express = require('express');
const bodyParser = require('body-parser');

const secret = 'YOUR_SECRET';
const app = express();

app.use(expressJwt({ secret }).unless({ path: ['/login'] }));
app.use(bodyParser.urlencoded());

// the private route you can reach on a GET method with POSTMAN (with and without the token)
app.get('/secured', (req, res) => {
  const securedInfos = {
    foo: 'bar',
    john: 'doe'
  }
  res.status(200).json(securedInfos);
});

// the public route you can reach on a POST method with POSTMAN to retrieve the token
app.post('/login', (req, res) => {
  console.log(req.body);
  const token = jwt.sign({ username: req.body.username }, secret);
  res.status(200).json(token);
});

app.listen(3000, () => console.log('listening on 3000'));
```

