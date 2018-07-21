# Steps to be followed to add an Angular app from local git repo to Github

- Login to GitHub(create new account if you do not have one) and click on "Add Repository" to add a new repository.
- Name the repository "git-to-github-linkage" and click on create repository(DO NOT select creation of a new README.md file option).
- Make sure the new GitHub repository is empty.
- In the local machine create new angular project using command "ng new angular-demo".(A local git repo is initialized automatically)
- Copy the GitHub repository URL and add a git remote in local machine using "git remote add origin github-repo-url.git"
- In the local machine run commands "git add" and "git commit".
- Push the local "master" branch to the remote repository using "git push origin master" (if prompted for github username and password provide the same).
