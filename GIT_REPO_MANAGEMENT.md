# Steps to remove and replace remote repo once we have cloned the code

1. For the remote repo from which we want to clone/get the code, select the https/ssh git url(by clicking on the 'Code' dropdown button).
2. Open command prompt/git bash on local machine and use:
  > git clone \<git-url> (to keep folder name as is for the cloned project)

  or 

  > git clone \<git-url> \<folder-name> (to clone code into the provided folder name)

3. Once the code is copied we can check the origin(remote repo git url) using:

  > git remote -v

This will give us the remote repo from which to pull code from/push code to.
Usually along with the git url the repo will also has a name such as 'origin'.

4. We need to remove the existing origin remote and add our own remote(since we may not have permission to check-in code to current remote
or want to make code changes in a repository that we own in GitHub)

For this use command:

  > git remote remove origin

OR

  > git remote rm origin

This command will remove the link of local repo that was copied from the remote github repo named 'origin'.
(Other remote repos other than 'origin' will still continue to be associated with this local repo)

5. To add a new remote first create a new repository in GitHub which you want to associate with the local repo copied in the first few steps.
Copy the URL for the new repo and then use the below command:

  > git remote add origin \<new-repo-git-url>

Now we can run:

  > git remote -v

This should show 'origin' associated with the new repo created in GitHub.

Now we can use commands such as:

  > git push origin master(to push changes in local 'master' branch to the remote 'origin')
