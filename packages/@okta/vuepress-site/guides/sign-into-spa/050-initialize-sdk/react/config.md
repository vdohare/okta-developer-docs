Configure a Security component in your top-level component, 'App.jsx'. The Security component will create a child context which exposes the Auth object to child components.

```javascript


import React, { Component } from 'react';
import { BrowserRouter as Router } from 'react-router-dom';
import { Security } from '@okta/okta-react';

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

If you find that your application needs to interact with the Auth object outside of a child component context, you can create the Auth object directly and pass it to the Security component as shown below.

Note that Auth has a dependency on the 'history' npm module. You should create this history object and pass it as a property to the Auth construxtor object as shown:

```javascript

import { Auth } from '@okta/okta-react';
import createHistory from 'history/createBrowserHistory';

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