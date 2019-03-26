For most applications, you will configure and create a Security component in your top-level component, 'App.jsx'. The Security component will create a child context which exposes the Auth object to child components.

```javascript


import React, { Component } from 'react';
import { BrowserRouter as Router } from 'react-router-dom';
import { Security } from '@okta/okta-react';


const config = {
  clientId: '{clientId}',
  issuer: 'https://{yourOktaDomain}.com/oauth2/default',
  redirectUri: 'http://localhost:8080/implicit/callback',
};


class App extends Component {
  render() {
    return (
      <Router>
        <Security {...config} >
          { /* App routes go here */ }
        </Security>
      </Router>
    );
  }
}

export default App;
```

If your application needs to use or check auth outside of components (for example in Redux actions), you can create the Auth object directly and pass it to the Security component.

Somewhere near the entry point of your application, build an Auth object. This object can be passed to the Security component and used by other areas of your application.

Note that Auth has a dependency on the 'history' npm module.

```javascript

import { Auth } from '@okta/okta-react';
import createHistory from 'history/createBrowserHistory';

const config = {
  clientId: '{clientId}',
  issuer: 'https://{yourOktaDomain}.com/oauth2/default',
  redirectUri: 'http://localhost:8080/implicit/callback',
};

const auth = new Auth({
  history,
  ...config
});

import React, { Component } from 'react';
import { BrowserRouter as Router } from 'react-router-dom';
import { Security } from '@okta/okta-react';

class App extends Component {
  render() {
    const { auth } = this.props;
    return (
      <Router>
        <Security auth={auth} >
          { /* App routes go here */ }
        </Security>
      </Router>
    );
  }
}

ReactDOM.render(<App auth={auth} />, document.getElementById('root'));
```