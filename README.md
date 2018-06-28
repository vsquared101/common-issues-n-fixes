# AngularDemo

This project was generated with [Angular CLI](https://github.com/angular/angular-cli) version 6.0.8.

## Steps followed to add this project from local git repo to Github

- Login to GitHub(create new account if you do not have one) and click on "Add Repository" to add a new repository.
- Name the repository "git-to-github-linkage" and click on create repository(DO NOT select creation of a new README.md file option).
- Make sure the new GitHub repository is empty.
- In the local machine create new angular project using command "ng new angular-demo".
- Copy the GitHub repository URL and add a git remote in local machine using "git remote add <remote-name> <github-remote-url>"
- In the local machine run commands "git add" and "git commit".
- Push the local "master" branch to the remote repository using "git push <remote-name> master" (if prompted for github username and password provide the same).

## Development server

Run `ng serve` for a dev server. Navigate to `http://localhost:4200/`. The app will automatically reload if you change any of the source files.

## Code scaffolding

Run `ng generate component component-name` to generate a new component. You can also use `ng generate directive|pipe|service|class|guard|interface|enum|module`.

## Build

Run `ng build` to build the project. The build artifacts will be stored in the `dist/` directory. Use the `--prod` flag for a production build.

## Running unit tests

Run `ng test` to execute the unit tests via [Karma](https://karma-runner.github.io).

## Running end-to-end tests

Run `ng e2e` to execute the end-to-end tests via [Protractor](http://www.protractortest.org/).

## Further help

To get more help on the Angular CLI use `ng help` or go check out the [Angular CLI README](https://github.com/angular/angular-cli/blob/master/README.md).
