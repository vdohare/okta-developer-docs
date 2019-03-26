In your application's `module.ts` file, import the following objects and create a configuration object. Your application module should provide this config using the OKTA_CONFIG injection token.

```javascript

import {
  OKTA_CONFIG,
  OktaAuthModule,
} from '@okta/okta-angular';

const config = {
  clientId: '{clientId}',
  issuer: 'https://{yourOktaDomain}.com/oauth2/default',
  redirectUri: 'http://localhost:8080/implicit/callback',
};

@NgModule({
  imports: [
    OktaAuthModule,
  ],
  providers: [
    { provide: OKTA_CONFIG, useValue: config },
  ],
})


```