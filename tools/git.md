| Command          | Example Usage                 | What it does                                                                 |
|------------------|-------------------------------|-----------------------------------------------------------------------------|
| `git init`       | `git init`                    | Initializes a new Git repository in the current directory.                    |
| `git clone`      | `git clone <repository_url>`  | Creates a copy of a remote repository on your local machine.               |
| `git add`        | `git add <filename>`          | Stages changes in a specific file for the next commit.                       |
|                  | `git add .`                   | Stages all changes in the current directory and its subdirectories.          |
| `git commit`     | `git commit -m "Your message"` | Saves the staged changes with a descriptive message.                         |
| `git status`     | `git status`                  | Shows the working tree status, indicating staged, unstaged, and untracked files. |
| `git log`        | `git log`                     | Displays a history of commits, including author, date, and message.          |
|                  | `git log --oneline`           | Shows a more concise, single-line history of commits.                       |
| `git branch`     | `git branch`                  | Lists all local branches in your repository.                                 |
|                  | `git branch <new_branch_name>`| Creates a new branch with the specified name.                               |
|                  | `git branch -d <branch_name>` | Deletes a local branch (only if it has been merged).                        |
|                  | `git branch -D <branch_name>` | Forces deletion of a local branch, even if it hasn't been merged.            |
| `git checkout`   | `git checkout <branch_name>`  | Switches to the specified branch.                                            |
|                  | `git checkout -b <new_branch_name>` | Creates a new branch and switches to it.                                 |
|                  | `git checkout -- <filename>`  | Discards changes in a specific file, reverting it to the last commit.        |
| `git merge`      | `git merge <branch_to_merge>` | Integrates changes from the specified branch into the current branch.         |
| `git remote`     | `git remote -v`               | Lists the names and URLs of configured remote repositories.                  |
|                  | `git remote add origin <repository_url>` | Adds a new remote repository named "origin".                           |
| `git push`       | `git push origin <branch_name>` | Sends your local commits to the remote repository and branch.               |
|                  | `git push -u origin <branch_name>` | Sets up tracking information for the branch, so future pushes can be done with `git push`. |
| `git pull`       | `git pull origin <branch_name>` | Fetches changes from the remote repository and merges them into your current branch. |
| `git fetch`      | `git fetch origin`            | Downloads commits and objects from the remote repository without merging them. |
| `git reset`      | `git reset HEAD <filename>`   | Unstages a file, but keeps its content.                                    |
|                  | `git reset --hard <commit_hash>` | Resets your repository to a previous commit, discarding all changes since then (use with caution!). |
| `git stash`      | `git stash`                   | Temporarily saves your uncommitted changes.                                  |
|                  | `git stash save "description"`| Saves your uncommitted changes with a description.                           |
|                  | `git stash list`              | Lists your stashed changes.                                                  |
|                  | `git stash apply`             | Reapplies the most recent stashed changes.                                   |
|                  | `git stash apply stash@{<n>}`| Applies a specific stashed change (where `<n>` is the stash index).          |
|                  | `git stash pop`               | Applies the most recent stashed changes and removes it from the stash list.   |
| `git diff`       | `git diff`                    | Shows changes between your working directory and the staging area.            |
|                  | `git diff --staged`           | Shows changes between the staging area and the last commit.                 |
|                  | `git diff <branch1> <branch2>`| Shows the differences between two branches.                                 |