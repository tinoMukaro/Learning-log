# Git Notes

## Configuration

```bash
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
git config --global core.editor "code --wait"
git config --global init.defaultBranch main
```

## basic flow

```bash
git init                  # Initialize repo
git clone <url>           # Clone remote repo
git status                # Check file status
git add <file>            # Stage file(s)
git add .                 # Stage all changes
git commit -m "msg"       # Commit staged changes
git commit -am "msg"      # Stage and commit tracked files
git log --oneline         # Compact commit history
```

## branching

git branch # List branches
git branch <name> # Create branch
git checkout <name> # Switch branch
git checkout -b <name> # Create + switch
git merge <branch> # Merge branch into current
git branch -d <name> # Delete branch

## Remote

```bash
git remote add origin <url>   # Link remote
git remote -v                  # View remotes
git push -u origin main        # Push + set upstream
git push                       # Push to upstream
git pull                       # Fetch + merge
git fetch                      # Fetch without merge

```

## undoing changes

```bash
git restore <file>          # Discard unstaged changes
git restore --staged <file> # Unstage file (keep changes)
git reset HEAD~1            # Undo last commit (keep changes)
git reset --hard HEAD~1     # Undo last commit (lose changes)
git revert <commit-hash>    # Create new commit undoing that one
```

## history

```bash
git log --graph --oneline --decorate   # Pretty graph log
git diff                 # Unstaged changes
git diff --staged        # Staged changes
git diff main..feature   # Between branches
git blame <file>         # Who changed each line
```
