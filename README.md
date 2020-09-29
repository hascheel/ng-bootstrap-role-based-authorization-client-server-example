# Angular 10 with Bootstrap - Role Based Authorization Example with Client & Server

An example of how to implement role based authorization / access control in Angular 10. Using JWT authentication with refresh tokens and an Node.js API back-end.

There are three pages - a login page, a home page and an admin page. There are two roles - a regular user (Role.User) that can access the home page, and an admin user (Role.Admin) that can access everything.

### Table of content
* [Building the client](#Building-the-client)
  * [Used technologies and components](#Used-technologies-and-components)
  * [Project structure](#Project-structure)
  * [Implementation](#Implementation)
  * [Documentation](#Documentation)
    * [TypeScript tsconfig.json](#TypeScript-tsconfig.json)
    * [Further information](#Further-information)
  * [Deployment](#Deployment)
* [Building the server](#Building-the-server)

---

# Building the client

## Used technologies and components
* For styling: [Bootstrap 4.5 CSS](https://getbootstrap.com/docs/4.5/getting-started/introduction/)

## Project structure

Start a new Angular 10 project with routing module:
```shell
$ ng new ng-bootstrap-role-based-auth-client
```

The `index.ts` files in each folder are barrel files that group the exported modules from a folder together so they can be imported using the folder path rather than the full module path, and to enable importing multiple modules in a single import (e.g. `import { AuthenticationService, UserService } from '../_services'`).

Path aliases `@app` and `@environments` have been configured in `tsconfig.base.json` that map to the `/src/app` and `/src/environments` directories. This allows imports to be relative to the app and environments folders by prefixing import paths with aliases instead of having to use long relative paths (e.g. `import MyComponent from '../../../MyComponent'`).

## TypeScript tsconfig.json

**Path: /tsconfig.json**

The paths property has been added to map the `@app` and `@environments` aliases to the `/src/app` and `/src/environments` directories. This allows imports to be relative to the app and environments folders by prefixing import paths with aliases instead of having to use long relative paths (e.g. `import MyComponent from '@app/MyComponent'` instead of `import MyComponent from '../../../MyComponent'`).

## Disable fake back-end

The fake back-end makes it possible to use the client serverless in a browser for easier development.

When disabling the fake back-end the URL in `/src/environments/environment.prod.ts` and `/src/environments/environment.ts`is used to call an external api.

To disable the fake back-end remove or comment out the line below the comment `// provider used to create fake backend` located in the `/src/app/app.module.ts`.

## Implementation

**/_helpers/**:
* add auth.guard.ts
* add error.interceptor.ts
* fake-backend.ts
* jwt.interceptor.ts

**/_models/**:
* role.ts
* user.ts

**/_services/**:
* authentication.service.ts
* user.service.ts

**Components**:
* admin
  ```shell
  $ ng generate component admin
  ```

* home
  ```shell
  $ ng generate component home
  ```

* login
  ```shell
  $ ng generate component login
  ```

**configure AppRoutingModule**

**configure AppComponentTemplate**

**configure AppComponent**

**configure AppModule**

# Documentation

Set up your local development environment: [See Jason Watmore's article](https://jasonwatmore.com/post/2020/06/02/angular-setup-development-environment)

## Further information
This client example app was built with the help of the following lecture:
* [Jason Watmore: role base authorization tutorial](https://jasonwatmore.com/post/2020/09/09/angular-10-role-based-authorization-tutorial-with-example) / [PDF version](./docs/documents/Jason-Watmore-Angular-10-Role-Based-Authorization-Tutorial-with-Example.pdf)
* [Jason Watmore: GitHub repo with the authorization example](https://github.com/cornflourblue/angular-10-role-based-authorization-example)

# Deployment

Install GitHub pages Deployment for Angular:
```shell
$ ng add angular-cli-ghpages
```

Deploy to my GitHub Pages hosting service:
```shell
$ ng deploy --base-href=https://scheel.dev/ng-bootstrap-role-based-authorization-client-server/
```

---

# Building the server

This is a simple example of how to implement a role based authorization / access control in a Node.js API.

The example API has just three endpoints to demonstrate authentication and role based authorization:

* `/users/authenticate` - public route that accepts HTTP POST requests with username and password in the body. If the username and password are correct then a JWT authentication token is returned.
* `/users` - secure route restricted to "Admin" users only, it accepts HTTP GET requests and returns a list of all users if the HTTP Authorization header contains a valid JWT token and the user is in the "Admin" role. If there is no auth token, the token is invalid or the user is not in the "Admin" role then a 401 Unauthorized response is returned.
* `/users/:id` - secure route restricted to authenticated users in any role, it accepts HTTP GET requests and returns the user record for the specified "id" parameter if authorization is successful. Note that "Admin" users can access all user records, while other roles (e.g. "User") can only access their own user record.

## Further information
This server example app was built with the help of the following lecture:
* [Jason Watmore: Node.js - Role Based Authorization Tutorial](https://jasonwatmore.com/post/2018/11/28/nodejs-role-based-authorization-tutorial-with-example-api) / [PDF version](./docs/documents/Jason-Watmore-Node.js-Role-Based-Authorization-Tutorial-with-Example-API.pdf)
* [Jason Watmore: GitHub repo with the authorization api](https://github.com/cornflourblue/node-role-based-authorization-api)
