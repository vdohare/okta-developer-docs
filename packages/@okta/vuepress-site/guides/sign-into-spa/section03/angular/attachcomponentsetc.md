## Attach Components to Routes
You need to provide these routes in your sample application so that we can sign the user in and handle the callback from Okta. We show you how to set these up below using [Angular Router](https://angular.io/guide/router):

- `/`: A default home page to handle basic control of the app.
- `/implicit/callback`: Handle the response from Okta and store the returned tokens.

```javascript

import { Routes, RouterModule } from '@angular/router';
import {
  OktaCallbackComponent,
} from '@okta/okta-angular';

const appRoutes: Routes = [
  {
    path: '',
    component: HomeComponent,
  },
  {
    path: 'implicit/callback',
    component: OktaCallbackComponent,
  },
];

@NgModule({
  imports: [
    RouterModule.forRoot(appRoutes),
  ],
})

```
## Provide the Login and Logout Buttons

In the relevant location in your application, you want to provide `Login` and `Logout` buttons for the user. The oktaAuth service can be injected in your component and you can use the `oktaAuth.isAuthenticated()` method to show/hide the correct button. For example:

```javascript
import { Component } from '@angular/core';
import { OktaAuthService } from '@okta/okta-angular';

@Component({
  selector: 'app-root',
  template: `
    <button *ngIf="!isAuthenticated" (click)="login()"> Login </button>
    <button *ngIf="isAuthenticated" (click)="logout()"> Logout </button>
    <router-outlet></router-outlet>
  `,
})
export class AppComponent {
  isAuthenticated: boolean;

  constructor(public oktaAuth: OktaAuthService) {
    // Subscribe to authentication state changes
    this.oktaAuth.$authenticationState.subscribe(
      (isAuthenticated: boolean)  => this.isAuthenticated = isAuthenticated
    );
  }

  async ngOnInit() {
    // Get the authentication state for immediate use
    this.isAuthenticated = await this.oktaAuth.isAuthenticated();
  }

  login() {
    this.oktaAuth.loginRedirect('/profile');
  }

  logout() {
    this.oktaAuth.logout('/');
  }
}
```

## Create the Callback Handler

In order to handle the redirect back from Okta, you need to capture the token values from the URL. Use `/implicit/callback` as the callback URL and specify the default `OktaCallbackComponent` and declare it in your `NgModule`.


```javascript

import { Routes, RouterModule } from '@angular/router';
import {
  OktaCallbackComponent,
} from '@okta/okta-angular';

const appRoutes: Routes = [
  {
    path: '',
    component: HomeComponent,
  },
  {
    path: 'implicit/callback',
    component: OktaCallbackComponent,
  },
];

@NgModule({
  imports: [
    RouterModule.forRoot(appRoutes),
  ],
})

```

## Update Your NgModule

Finally, import the `OktaAuthModule` into your `NgModule` and provide the config object.

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