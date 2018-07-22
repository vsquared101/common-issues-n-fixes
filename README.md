# Steps to be followed to add an Angular app from local git repo to Github

- Login to GitHub(create new account if you do not have one) and click on "Add Repository" to add a new repository.
- Name the repository "git-to-github-linkage" and click on create repository(DO NOT select creation of a new README.md file option).
- Make sure the new GitHub repository is empty.
- In the local machine create new angular project using command "ng new angular-demo".(A local git repo is initialized automatically)
- Copy the GitHub repository URL and add a git remote in local machine using "git remote add origin github-repo-url.git"
- In the local machine run commands "git add" and "git commit".
- Push the local "master" branch to the remote repository using "git push origin master" (if prompted for github username and password provide the same).

# Issue with security vulnerability after pushing code to GitHub

## IF after pushing the Angular code for the very first time we see a message related to security vulnerabilities detected in the code specifically for 'hoek' npm package we can follow the below steps to resolve it:

- As of today(22nd July 2018) the latest version of hoek is 5.0.3 so we can go the root of our Angular project and run "npm install hoek --save" to install the latest version, alternatively we can also provide the version using @<version_no> along with the package name.
- Once installed we will see that the package.json and package-lock.json files in our project are updated.
- Add and commit these files in the local git repo and then use "git push origin master" to push the changes in the local master branch to the remote origin repository.
- Refresh the repository page in GitHub and the security vulnerability related warning message should have disappeared.
