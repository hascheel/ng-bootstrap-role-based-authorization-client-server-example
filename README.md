# Angular 10 with Bootstrap - Role Based Authorization Example with Client & Server

This is an implementaton of a role based authorization in Angular with a Node.js API from a tutorial.

There are three pages - a login page, a home page and an admin page. There are two roles - a regular user (Role.User) that can access the home page, and an admin user (Role.Admin) that can access everything.

There is no database. A fake backend with hardcoded data in the backend answers the API calls.

### Table of content
* [Building the client](#building-the-client)
  * [Used technologies and components](#used-technologies-and-components)
  * [Project structure](#project-structure)
  * [Implementation](#implementation)
  * [Documentation](#documentation)
    * [TypeScript tsconfig.json](#typeScript-tsconfig.json)
    * [Further information](#further-information)
  * [Deployment](#deployment)
* [Building the server](#building-the-server)
  * [Project structure](#project-structure)
  * [Authorize Role Middleware](#authorize-role-middleware)
  * [Auth Global Error Handler Middleware](#auth-global-error-handler-middleware)
  * [Auth Role Object / Enum](#auth-role-object-/-enum)
  * [Auth User Service](#auth-user-service)
  * [Auth Users Controller](#auth-users-controller)
  * [Auth App Config](#auth-app-config)
  * [Auth Main Server Entrypoint](#auth-main-server-entrypoint)
  * [Further information](#further-information)
  * [Original Licence](#original-licence)

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

## Project structure

**Path: /_helpers**
The helpers folder contains that are used to help the app to work.

## Authorize Role Middleware

**Path: /_helpers/authorize.js**

The authorize middleware can be added to any route to restrict access to authenticated users within specified roles. If the roles parameter is left blank then the route will be restricted to any authenticated user regardless of role. It's used in the users controller to restrict access to the "get all users" and "get user by id" routes.

The authorize function actually returns 2 middleware functions, the first (`jwt({ ... })`) authenticates the request by validating the JWT token in the Authorization http request header. On successful authentication a `user` object is attached to the `req` object that contains the data from the JWT token, which in this case includes the user id (`req.user.sub`) and user role (`req.user.role`). The `sub` property is short for subject and is the standard JWT property for storing the id of the item in the token.

The second middleware function checks that the authenticated user is authorized to access the requested route based on their role. If either authentication or authorization fails then a 401 Unauthorized response is returned.

## Auth Global Error Handler Middleware

**Path: /_helpers/error-handler.js**

The global error handler is used catch all errors and remove the need for redundant error handler code throughout the application. It's configured as middleware in the main server.js file.

## Auth Role Object / Enum

**Path: /_helpers/role.js**

The role object defines the all the roles in the example application. It is created so to use it like an enum to avoid passing roles around as strings, so instead of `'Admin'` we can use `Role.Admin`.

## Auth User Service

**Path: /users/user.service.js**

The user service contains a method for authenticating user credentials and returning a JWT token, a method for getting all users in the application, and a method for getting a single user by id.

The array of users are hardcoded. In a production application it is recommended to store user records in a database with hashed passwords.

Near the top of the file (below the hardcoded users) are the exported service method definitions so it's easy to see all methods at a glance, and below that the rest of the file contains the method implementations.

## Auth Users Controller

**Path: /users/users.controller.js**

The users controller defines all user routes for the api, the route definitions are grouped together at the top of the file and the route implementations are below.

Routes that use the authorize middleware are restricted to authenticated users, if the role is included (e.g. `authorize(Role.Admin)`) then the route is restricted to users in the specified role / roles, otherwise if the role is not included (e.g. `authorize()`) then the route is restricted to all authenticated users regardless of role. Routes that don't use the authorize middleware are publicly accessible.

The `getById` route contains some extra custom authorization logic within the route function. It allows admin users to access any user record, but only allows normal users to access their own record.

## Auth App Config

**Path: /config.json**

The app config file contains configuration data for the api.

IMPORTANT: The "secret" property is used by the api to sign and verify JWT tokens for authentication, update it with your own random string to ensure nobody else can generate a JWT to gain unauthorised access to your application.

## Auth Main Server Entrypoint

**Path: /server.js**

The server.js file is the entry point into the api, it configures application middleware, binds controllers to routes and starts the Express web server for the api.

## Further information

This server example app was built with the help of the following lecture:
* [Jason Watmore: Node.js API - Role Based Authorization Tutorial](https://jasonwatmore.com/post/2018/11/28/nodejs-role-based-authorization-tutorial-with-example-api) / [PDF version](./docs/documents/Jason-Watmore-Node.js-Role-Based-Authorization-Tutorial-with-Example-API.pdf)
* [Jason Watmore: GitHub repo with the authorization api](https://github.com/cornflourblue/node-role-based-authorization-api)

## About
This is an implementaton of a role based authorization in Angular with a Node.js API with the help of a tutorial from [Jason Watmore](https://jasonwatmore.com/post/2018/11/28/nodejs-role-based-authorization-tutorial-with-example-api).

## Original Licence

The original version of this software can be found here:
* [Angular 10 - Role Based Authorization Tutorial with Example](https://github.com/cornflourblue/angular-10-role-based-authorization-example)
* [Node.js - Role Based Authorization Tutorial with Example API](https://github.com/cornflourblue/node-role-based-authorization-api)

```
The MIT License (MIT)

Copyright (c) 2018 Jason Watmore

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```
