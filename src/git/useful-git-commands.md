# Useful Git Commands
## `git log --oneline`

- `git log --oneline` can be used to see the list of commits in a repo
## `git reset --hard HEAD~1`
- Used to reset the most recent commit.
- All staged and unstaged changes are **destroyed**.

## `git reset --soft HEAD~1`
- All changes from the removed commit remain staged
- But commit goes back by 1
## git stash
- If you are working on a  branch and you need to switch to another branch to work on something else, then git will not allow you to do so if you have uncommittedd changes.
- In such a situation, `git stash` can be used to temporarily save uncommitted changes. It reverts your directory to the last committed state.
> Use git stash when you want to record the current state of the working directory and the index, but want to go back to a clean working directory. The command saves your local modifications away and reverts the working directory to match the HEAD commit.

- Use `git stash pop` when you are ready to add the changes back to your code


## git commit -am
- Instead of using `git add .` then `git commit -m "commit message"`, you can use 

    `git commit -am "message"`
    
## View git remote URL
- `git remote -v` to view the remote URLs of a repository.

