# Angular 10 with Bootstrap - Role Based Authorization Example

An example of how to implement role based authorization / access control in Angular 10. Using JWT authentication with refresh tokens and a fake backend.

There are three pages - a login page, a home page and an admin page. There are two roles - a regular user (Role.User) that can access the home page, and an admin user (Role.Admin) that can access everything.

### Table of content
* [Building the client](#Building-the-client)
  * [Used technologies and components](#Used-technologies-and-components)
  * [Project structure](#Project-structure)
  * [Implementations](#Implementations)
  * [Documentation](#Documentation)
    * [TypeScript tsconfig.json](#TypeScript-tsconfig.json)
    * [Further information](#Further-information)
  * [Deployment](#Deployment)

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

## Implementations

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
This example app was built with the help of the following lecture:
* [Jason Watmore: role base authorization tutorial](https://jasonwatmore.com/post/2020/09/09/angular-10-role-based-authorization-tutorial-with-example) / [PDF version](./docs/documents/Jason-Watmores-Blog-Angular-10-Role-Based-Authorization-Tutorial-with-Example.pdf)
* [Jason Watmore: GitHub repo with the authorization example](https://github.com/cornflourblue/angular-10-role-based-authorization-example)

# Deployment

```shell
$ ng deploy --base-href=https://scheel.dev/ng-bootstrap-role-based-authorization-example/
```
