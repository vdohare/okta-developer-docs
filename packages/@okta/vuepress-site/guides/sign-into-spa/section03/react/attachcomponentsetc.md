## Attach Components to the Secure Router

We provide the `Security` component which makes an Auth object available to child components and the `withAuth` HOC. We show you how to set these up below using [React Router DOM](https://github.com/ReactTraining/react-router/tree/master/packages/react-router-dom):

```javascript
import React, { Component } from 'react';
import { BrowserRouter as Router, Route } from 'react-router-dom';
// import config from ...

class App extends Component {
  render() {
    return (
      <Router>
        <Security {...config} >
          { /* child components have access to auth context */ }
        </Security>
      </Router>
    );
  }
}

export default App;
```

## Provide the Login and Logout Buttons

In the relevant location in your application, you want to provide `Login` and `Logout` buttons for the user. You can show/hide the correct button by using the `auth.isAuthenticated()` method.  In this example,`props.auth` has been provided via the `withAuth` HOC.

```javascript

import { withAuth } from '@okta/okta-react';
import React, { Component } from 'react';

async function checkAuthentication() {
  const authenticated = await this.props.auth.isAuthenticated();
  if (authenticated !== this.state.authenticated) {
    if (authenticated && !this.state.userinfo) {
      const userinfo = await this.props.auth.getUser();
      this.setState({ authenticated, userinfo });
    } else {
      this.setState({ authenticated });
    }
  }
}

export default withAuth(class Home extends Component {
  constructor(props) {
    super(props);
    this.state = { authenticated: null, userinfo: null };
    this.checkAuthentication = checkAuthentication.bind(this);
    this.login = this.login.bind(this);
  }

  async componentDidMount() {
    this.checkAuthentication();
  }

  async componentDidUpdate() {
    this.checkAuthentication();
  }

  async login() {
    this.props.auth.login('/');
  }

  render() {
    return (
      <div>
        {this.state.authenticated !== null &&
        <div>
          {this.state.authenticated &&
            <div>
              <p>Welcome back, {this.state.userinfo.name}!</p>
            </div>
          }
          {!this.state.authenticated &&
            <div>
              <a onClick={this.login}>Login</a>
            </div>
          }
        </div>
        }
      </div>
    );
  }
});
```

## Create the Callback Handler

In order to handle the redirect back from Okta, you need to capture the token values from the callback URL `/implicit/callback` and pass them to the handleAuthentication() method of the Auth object. We have provided `ImplicitCallback` which implements this logic. It can be mapped to a route as shown:


```javascript
import React, { Component } from 'react';
import { BrowserRouter as Router, Route } from 'react-router-dom';
import { Security, ImplicitCallback } from '@okta/okta-react';
// import config from ...

class App extends Component {
  render() {
    return (
      <Router>
        <Security {...config} >
            <Route path="/implicit/callback" component={ImplicitCallback} />
        </Security>
      </Router>
    );
  }
}

```
