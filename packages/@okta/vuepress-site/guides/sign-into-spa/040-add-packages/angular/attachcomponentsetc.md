## Provide the Login and Logout Buttons

In the relevant location in your application, you want to provide `Login` and `Logout` buttons for the user. The `OktaAuthService` is injected in your component's constructor. You can use the `oktaAuth.isAuthenticated()` method to show/hide the correct button. For example:

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

In order to handle the redirect back from Okta, you need to capture the token values from the callback URL `/implicit/callback` and pass them to the handleAuthentication() method of the OktaAuthService. We have provided `OktaCallbackComponent` which implements this logic.  We show you how to set this up below using [Angular Router](https://angular.io/guide/router):

```javascript
//...
import {
  OktaCallbackComponent,
} from '@okta/okta-angular';

const appRoutes: Routes = [
  // ...
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

