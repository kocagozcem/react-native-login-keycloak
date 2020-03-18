# react-native-login-keycloak
This is a fork of jonatasdlp react-native-login-keycloak module. This version implements auth process without browser linking.

## Usage

### App configuration

Please configure [Linking](https://facebook.github.io/react-native/docs/linking.html) module, including steps for handling Universal links (This might get changed due to not being able to close the tab on leave, ending up with a lot of tabs in the browser).

Also, add the applinks:<APPSITE HOST> entry to the Associated Domains Capability of your app.


### Imports

```js
import Login from 'react-native-login-keycloak';
```

### Checking if tokens are saved on the device

```js
const gatheredTokens = await Login.getTokens();
console.log(gatheredTokens);

// Prints:
//
// { access_token: '...', refresh_token: '...', id_token: '...', ...}
// OR
// undefined
```

### Login
```js

const config = {
  url: 'https://<KEYCLOAK_HOST>/auth',
  realm: '<REALM NAME>',
  clientId: '<CLIENT ID>',
  username: '<USERNAME>',
  password: '<PASSWORD>',
  clientSecret: '<CLIENT_SECRET>',
  kcIdpHint: 'facebook', // *optional*
};

Login.startLoginProcess(config).then(tokens => {
  console.log(tokens);
});

// Prints:
//
// { access_token: '...', refresh_token: '...', id_token: '...', ...}
```

Logging in by the startLoginProcess function will save it in the AsyncStorage, whereas after its been successful, getTokens will get the most recent tokens that are saved and you can then use it to authenticate against a backend.

### Refreshing the token
```js
const refreshedTokens = await Login.refreshToken();
console.log(refreshTokens);
// Prints:
//
// { access_token: '...', refresh_token: '...', id_token: '...', ...}
// OR
// undefined
```



### Retrieving logged in user info
```js
const loggedInUser = await Login.retrieveUserInfo();
console.log(loggedInUser);

// Prints:
//
// { sub: '...',name: '... ',preferred_username: '...',given_name: '...' }

// OR
// undefined
```


### Logout

```js
Login.logoutKc();
```
Removes stored tokens. Will also do a Keycloak call to log the user out. Returns true on logout, else false. Subsequent calls to Login.tokens() will return null.

If you got any improvements feel free to make a pull request or suggestion.
