# Git essentials

```bash
# Create an empty Git repository in the current directory
git init

# Download a remote repository and its history into a new local directory
git clone <repository-url>

# Show the current branch and staged, unstaged, and untracked changes
git status

# Show unstaged changes in tracked files
git diff

# Show changes already added to the staging area; --staged is the index-versus-HEAD diff
git diff --staged

# Add one file's current changes to the staging area
git add <file>

# Stage changes below the current directory, including additions and deletions
git add .

# Record staged changes with a message; -m supplies the message on the command line
git commit -m '<message>'

# Show a compact decorated commit graph for every branch
git log --oneline --graph --decorate --all

# List local branches and mark the current branch
git branch

# Create a branch and switch to it; -c means create
git switch -c <branch-name>

# Switch the working tree to an existing branch
git switch <branch-name>

# Merge the named branch into the currently checked-out branch
git merge <branch-name>

# List remote names together with their fetch and push URLs; -v means verbose
git remote -v

# Fetch the current branch's upstream changes and integrate them locally
git pull

# Upload local commits to the current branch's configured upstream
git push

# Push a branch to origin and set its upstream tracking branch; -u means set upstream
git push -u origin <branch-name>
```

Review `git status`, `git diff`, and `git diff --staged` before committing.
