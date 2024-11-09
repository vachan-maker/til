# Push an existing local repository to GitHub
How to push an existing local repository to GitHub

**Make sure the default branch name is 'main'.(main is the default branch name in my case.)**

1. Create a new repo on GitHub. The repository should be empty.
2. Copy the URL of the repo on GitHub
3. Add the copied URL as remote URL.
   
    `git remote add origin REMOTE-URL`

4. Push the repo 
   
    `git push origin main`