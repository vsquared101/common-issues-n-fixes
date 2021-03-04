# Steps to work with imported Maven project with proxy settings/automatic configuration script

- In Eclipse go to Window -> Preferences -> Network Connections and set the Active Provider to "Native" and check only the HTTP,Dynamic,Dynamic,Native,No checkbox.
- Go to Internet Explorer -> Connections -> LAN Settings and get the details or path of the automatic configuration script or the proxy host and port no. if directly provided.
- Open the file corresponding to the automatic configuration script(can be a file with .pac or .dat extension) and make a note of the HTTP host and port values.
- In settings.xml file inside ".m2" folder add the below script inside the `proxies` tag.(remove id, username, password tags if not needed or values are not available)

```xml

<proxy>
      <active>true</active>
      <protocol>http</protocol>
      <host>someurl.companyname.com</host>
      <port>portno</port>
</proxy>

```
- Go to the imported maven project on the command line and run "mvn clean install" to download and install all project dependencies.
- If the java files within the project do not have Java nature add it to the .project file using below statement:

```xml

  <nature>org.eclipse.jdt.core.javanature</nature>

```

- Right-click the project in Eclipse and click on Maven -> Update Project to reflect the Java nature change.
- IF you see any issues with the download of a `particular` Maven dependency it may be because the Maven repository has become corrupt. Try to `remove` all files and folders inside the `m2` folder and see if it fixes the dependency download issue(MAKE SURE the proxy is `correctly` configured before trying this step.)


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

## Steps to install SDKMAN

1. Install SDKMAN:

  > curl -s "https://get.sdkman.io" | bash

2. Run initialize scripts:

  > source "/home/ubuntu/.sdkman/bin/sdkman-init.sh"

3. Get list of Gradle versions available with SDKMAN:

  > sdk list gradle

In the output of the above command, the version preceeded with:

+ => local gradle version
* => installed gradle version
> => gradle version currently in use.

4. Run below command to install gradle 10.2.0

  > sdk install gradle 10.2.0

5. Now run the "sdk list gradle" command. The output will have below version:

  > * 4.10.2 (which denotes that this is the version of gradle that is installed and currently in use)

6. Follow similar steps for Java:

  > sdk list java

-- Will display list of available jdk versions with SDKMAN.


### Available Java Versions

     12.ea.12-open                                                              
     11.ea.28-open                                                              
     10.0.2-zulu                                                                
     10.0.2-open                                                                
     10.0.2-oracle                                                              
     9.0.7-zulu                                                                 
     9.0.4-open                                                                 
     8.0.181-zulu                                                               
     8.0.181-oracle                                                             
     7.0.191-zulu                                                               
     6.0.113-zulu                                                               
     1.0.0-rc6-graal
	 
7. Now we can install a version from the above list using:

  > sdk install java 10.0.2-oracle

https://wpanas.github.io/tools/2017/12/25/sdkman.html

## Steps to setup a Java/Gradle/Spring Boot project in Cloud9

1. Create a new workspace with the blank template in cloud9.

2. Inside the workspace create a new folder named ‘projects’. ‘cd’ into the ‘projects’ directory.

3. Check the values set for git user.name and user.email using the ‘git config --list’ command.  (git is pre-installed in the Blank template of cloud9)

4. Set appropriate values using git config --global user.name <value-in-double-quotes> and git config --global user.email <value-in-double-quotes>

5. Checkout the project available in GitHub using the ‘git clone’ command.(e.g. git clone https://github.com/vsquared101/tms.git)

6. Install sdkman which we will need to install java and gradle in cloud9.

    a. Run the command > curl -s "https://get.sdkman.io" | bash

    b. Run the initialize scripts for sdkman > source "/home/ubuntu/.sdkman/bin/sdkman-init.sh"

    c. Run the command > sdk list gradle (to view available gradle versions)

    d. Run > sdk install gradle 4.10.2 (to install the provided version here)

    e. Run the command > sdk list java (to view available java versions)

    f. Run > sdk install java 8.0.181-oracle (to install the version provided here)

    g. Run > gradle –version (to verify the version of gradle that was installed)

    h. Run > java –version(to verify the version of java that was installed)

7. Run > gradle tasks (to view all the tasks available. If Spring Boot plugin is added to build.gradle file then a task named ‘bootJar’ will be available in the list of tasks)

8. Run > gradle bootJar (to assemble an executable jar archive containing the main classes and their dependencies inside the ‘build/libs/’ directory)

9. Run the Spring Boot application using ‘java –jar build/libs/<jar-file>’.(by default the application runs at port 8080)

## Steps to push an Angular project to GitHub Pages(TechStack: Node: 11.4.0, NPM: 6.5.0, Angular CLI: 7+, Bootstrap: 4.1+)

1. Create a project using 'ng new' command on the local machine and make ALL the necessary code changes.
(no need to add and commit to local git and we will be pushing/deploying only the files generated as part of 'ng build' command and not the source files)

2. Create an empty repository in GitHub with the name `<github-user-name>.github.io`(NO README.md file needed)
  
3. In the Angular project on local machine go to angular.json file and update "outputPath" under "build" to only "dist"(remove the project name after dist). Then run `ng build --prod` to build the code into the dist folder. 

4. In the Angular project root in local machine add the remote repository created in Step 2. (using 'git remote add origin `url`')
  
5. Use commands "git add ." and "git commit" to commit changes made to the angular.json file in step (3).

6. Remove "dist" folder entry from the gitignore file in project root on local machine(if dist entry is there) before pushing the code to remote github repo.

7. Command to push the code to GitHub pages:

  > git subtree push --prefix dist origin master

  (subtree allows us to push only the dist folder on the local master branch to the origin remote branch)

8. Navigate to the URL given in the repository settings under the published URL value.(should be `https://<github-user-name>.github.io/`)

## Eclipse short-cuts

- Use Ctrl + m/M to switch the editor window size

- Use upper case first letters of class for eg AL for ArrayList with Ctrl + Shift + R/T to open that class

- Alt + up arrow down arrow to move a piece of code up or down

- Use Ctrl + T to see the type hierarchy of the selected class

- Use Ctrl + Shift + L to see a list of all eclipse shortcuts

- Use Ctrl + Q to go to the last edit performed even if the edit was done in another class

- Use F3 to open a selected class name

- Use F4 to open type hierarchy 

- Toggle breadcrumbs  to see the complete path of the file that is open at the top of the edit screen

- Toggle Mark occurrences to mark all occurrences of a selected variable

- Use Alt + Shift + M to extract a method using selected lines of code

- Use Alt + Shift + I to inline the method/ put it back inside the other method

- Use Alt + Shift + T to open the refactor menu

- Use Alt + Shift + C to add parameter to a method/change signature of the method

- Use Alt + Shift + V to move a method from one class / type to another

- Use Alt + Shift + S to generate code

- To enable formatting of code on save and not selecting code and manually formatting it use Save actions which can remove unnecessary code, auto format code, etc on save 

- We can create working sets to group projects

## Useful IntelliJ Shortcuts

1. Path to set JDK:
```
File | Project Structure | Platform Settings | SDKs. 
```
2. 
```
(Alt + Insert) to create constructor, getters/setters, toString(), etc.
```
3. To extract expression into a variable use:
```
Alt + Ctrl + V
```
4. Remove unused imports:
```
Ctrl + Alt + O
```
5. To format code:
```
Ctrl + Alt + L
```
6. To create test class for a class:
```
Ctrl + Shift + T
```
7. For auto-import/suggestion for importing static imports for assertThat() type assertThat() and press:
```
Alt + Enter
```
- We should see "import static method..." come up which would give us different package suggestions.

8. Shift + Ctrl + space - to see intellisense/suggestions.

9. If we want to put some snipped of code into a private method 1st select the code and then click - Ctrl + Alt + M. We can provide method name in the pop-up box.

