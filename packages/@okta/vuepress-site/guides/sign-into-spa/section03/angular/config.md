Add OktaAuthModule module to the imports section of your application module's `@NgModule` declaration as shown below.

Your application module should provide a configuration object using the OKTA_CONFIG injection token. This value is required to initialize the OktaAuthService when it is created by the Angular dependency injection system. 


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