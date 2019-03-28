---
title: Config object
---

### Things You Need
After you create the application, there are two values that you need:

* **Client ID** - Find it in the applications list or on the **General** tab of a specific application.
* **Org URL** - Find it on the Developer console dashboard in the upper-right corner. 

These values are used in your application to set up the OpenID Connect flow with Okta.

In your app, construct a config object. This will be used to initialize the Okta services with the values specific to your application.

```javascript
const config = {
  clientId: '{clientId}',
  issuer: 'https://{yourOktaDomain}.com/oauth2/default',
  redirectUri: 'http://localhost:8080/implicit/callback',
};
```

We will refer to this `config` object in the next steps