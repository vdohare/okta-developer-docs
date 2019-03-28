---
title: Handle Callback
---
# Routing in SPA Application

Unlike traditional web applications where all HTML is rendered on a server and returned to the client, a Single Page Application (SPA) usually includes only a minimal amount of HTML returned from the server. The vast majority of HTML is rendered client-side and the DOM is updated dynamically using Javascript.

To create the illusion of multiple 'pages' while also maintaining support for browser reload on any 'page', most SPA applications define a set of 'routes' mapped from a path element or hash fragment of the URI.

On the server side, all paths which are handled by the SPA app are usually set in a wildcard fashion to return the same response, a static HTML document which loads the SPA application.

# Login Redirect Flow

1. User loads your SPA app at route X
1. User clicks login button or attempts to access to secure resource
2. User's browser is redirected to Okta's site for authentication
3. User successfully authentiates, Okta redirects the browser back to a specific `Login Redirect URI` which you have provided. 
4. This `Login Redirect URI` serves your SPA application
5. Information in the URI is processed and the user is redirected back to route X


## Login Redirect URI

If your SPA app is using a router, you should define a new 'route' where your app will be loaded and can handle this login redirect callback.

On successful authentication, an access token will be present in the hash fragment of this URI for your app to consume.

Ultimately, your app is responsible to read this token from the URI and pass it to the `handleAuthorization()` method, and to complete the flow by redirecting to the original route. 

Luckily, we have provided components which implement this logic for you!




