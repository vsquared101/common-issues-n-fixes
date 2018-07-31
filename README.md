# Steps to be followed to add an Angular app from local git repo to Github

- Login to GitHub(create new account if you do not have one) and click on "Add Repository" to add a new repository.
- Name the repository "git-to-github-linkage" and click on create repository(DO NOT select creation of a new README.md file option).
- Make sure the new GitHub repository is empty.
- In the local machine create new angular project using command "ng new angular-demo".(A local git repo is initialized automatically)
- Copy the GitHub repository URL and add a git remote in local machine using "git remote add origin github-repo-url.git"
- In the local machine run commands "git add" and "git commit".
- Push the local "master" branch to the remote repository using "git push origin master" (if prompted for github username and password provide the same).

# Issue with security vulnerability after pushing code to GitHub

## IF after pushing the Angular code for the very first time you see a message related to security vulnerabilities detected in the code specifically for 'hoek' npm package you can follow the below steps to resolve it:

- As of today(22nd July 2018) the latest version of hoek is 5.0.3 so you can go the root of the Angular project and run "npm install hoek --save" to install the latest version, alternatively you can also provide the version using @<version_no> along with the package name.
- Once installed you will see that the package.json and package-lock.json files in the project have been updated.
- Add and commit these files in the local git repo and then use "git push origin master" to push the changes in the local master branch to the remote origin repository.
- Refresh the repository page in GitHub and the security vulnerability related warning message should have disappeared.

## Creating Angular service which reads data from external json file:

1. Create a file named "users.json" with the below data under "src/assets" folder
(Note: The format and structure of this file is DIFFERENT from the one used with json-server to create a mock REST API.
 For json-server we need to use the format { "users" : <place the below provided users.json file content here>}. 
 This will give us endpoints such as /users, /users/:id, etc. for different HTTP verbs.)

```json
  [
      {
        "id": 1,
        "full_name": "Mr Charles Stewart",
        "short_name": "Charles Stewart",
        "ssn": "11-22-3333",
        "dob": "10-10-1968",
        "gender": "Male",
        "marital_status": "Married",
        "citizenship": "United States",
        "country_of_residence": "Australia",
        "passport": "H1234567",
        "country_of_issuance": "Australia",
        "issuance_date": "15-04-2010",
        "expiration_date": "15-04-2020",
        "no_of_dependents": 2
      },
      {
        "id": 2,
        "full_name": "Ms Diana Stewart",
        "short_name": "Diana Stewart",
        "ssn": "44-55-6666",
        "dob": "21-12-1977",
        "gender": "Female",
        "marital_status": "Married",
        "citizenship": "United States",
        "country_of_residence": "Australia",
        "passport": "M2231626",
        "country_of_issuance": "Australia",
        "issuance_date": "11-07-2012",
        "expiration_date": "11-07-2022",
        "no_of_dependents": 1
      },
      {
        "id": 3,
        "full_name": "Mr Phillip Stewart",
        "short_name": "Phil Stewart",
        "ssn": "55-66-7777",
        "dob": "09-04-1975",
        "gender": "Male",
        "marital_status": "Married",
        "citizenship": "United States",
        "country_of_residence": "Australia",
        "passport": "H2345678",
        "country_of_issuance": "Australia",
        "issuance_date": "25-03-2010",
        "expiration_date": "25-03-2020",
        "no_of_dependents": 3
      }
  ]
```

2. With our data in place we can now create a new service using "ng generate service user"
Open the newly created user.service.ts file and add the below content:

```typescript

  import { Injectable } from '@angular/core';
  import { HttpClient } from '@angular/common/http';

  @Injectable({
    providedIn: 'root'
  })
  export class UserService {

    constructor(private http: HttpClient) { }

    getUsers() {
      return this.http.get('assets/users.json'); 
    // note this will return an observable of "any" type that we need to subscribe to in the calling method.
    // we can also use './assets/users.json' above as it will also return the same data.
    }
  }

```

3. In the app.module.ts file we can add this service to the "providers" array in the @NgModule decorator.
   (This is NOT done automatically by Angular CLI when we create the service)
   
```typescript

 providers: [UserService]
 
```

4. In the component.ts file we can inject UserService into the "constructor" and then call the getUsers() method of the service inside ngOnInit() method:

```typescript

  constructor(private service: UserService) { }

  ngOnInit() {
      this.service.getUsers()
        .subscribe((data) => {
          //console.log(data); // UNCOMMENT this line if you want to see the data returned by the service in case of any issues
          this.users = data;
        });
  }

```

5. Now the data(array of users) will be available as the "users" property which can be referenced inside the template/HTML file:
(below code assumes that bootstrap 4 is already wired up with the Angular project)

```html

  <h2 class="dbrd-lbl">Dashboard</h2>
  <div class="table-responsive">
    <table class="table table-striped">
      <thead>
        <tr>
          <th>Name</th>
          <th>Country</th>
          <th>SSN/TIN</th>
          <th></th>
          <th></th>
          <th></th>
        </tr>
      </thead>
      <tbody>
        <tr *ngFor="let user of users">
          <td>{{ user.short_name}}</td>
          <td>{{ user.country_of_residence }}</td>
          <td>{{ user.ssn }}</td>
          <td><button [routerLink]="['/users', user.id, 'view']" class="btn btn-outline-dark btn-sm">View</button></td>
          <td><button [routerLink]="['/users', user.id, 'edit']" class="btn btn-outline-info btn-sm">Edit</button></td>
          <td><button class="btn btn-outline-danger btn-sm" (click)="deleteUser(user.id)">Delete</button></td>
        </tr>
      </tbody>
    </table>
  </div>
  <button class="btn btn-outline-info btn-xs" routerLink="/users/new">Create New User</button>

```
