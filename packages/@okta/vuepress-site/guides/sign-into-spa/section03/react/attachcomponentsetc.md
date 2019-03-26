## Attach Components to the Secure Router

You need to provide these routes in your sample application so that we can sign the user in and handle the callback from Okta. We show you how to set these up below using [React Router DOM](https://github.com/ReactTraining/react-router/tree/master/packages/react-router-dom):

- `/`: A default home page to handle basic control of the app.
- `/implicit/callback`: This is where auth is handled for you after redirection.

```javascript
import React, { Component } from 'react';
import { BrowserRouter as Router, Route } from 'react-router-dom';
import { Security, ImplicitCallback } from '@okta/okta-react';
import Home from './Home';

class App extends Component {
  render() {
    return (
      <Router>
        <Security {...config} >
            <Route path="/" exact component={Home} />
            <Route path="/implicit/callback" component={ImplicitCallback} />
        </Security>
      </Router>
    );
  }
}

export default App;
```

## Provide the Login and Logout Buttons

In the relevant location in your application, you want to provide `Login` and `Logout` buttons for the user. You can show/hide the correct button by using the `auth.isAuthenticated()` method. 

We provide a 'withAuth' Higher-Order-Component which allows child components to access the Auth object.

For example:

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

## Update Your App.js

Finally, pass in your configuration into `Security` and connect your application's paths:

{code}