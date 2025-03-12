# git-commands

We have already seen that git commits are just snapshots of an entire repository
at one point in time. In this hands on session we are going to explore some useful
git commands for helping keep a modular commit history.

There are a number of open PRs that show different branches which we will be using in this
demo. You can see the changes and commits in each branch by checking the PRs.

## Stashing local changes: git stash

- Start by cloning this repository
- Run `git status` to see you are on the main branch with no local changes.
- Add a line in `src/main.py`, e.g. add `print("test")` at the end of the main function.
- Run `git status` to see that `src/main.py` shows as changed
- Now we will try and merge the branch `stash-demo` into the branch `main`. The `stash-demo` branch
also makes a change to `src/main.py`. Do this by running `git merge stash-demo`.
- You will get an error like `error: Your local changes to the following files would be overwritten by merge: src/main.py`
Because your working directory is not clean git cannot do the merge. `git stash` provides a way to 
stash your local changes to a temporary location and retrieve them later.
- Run `git stash` to stash your changes to `src/main.py`.
- Now try again `git merge stash-demo`, this time the merge should complete. If you check `src/main.py`
you will see the changes introduced by merging the two branches but _not_ the change you made.
- Now run `git stash pop` to retrieve your changes to `src/main.py`. 
  - In `src/main.py` you will see the changes from `stash-demo` and the changes you made.
- Conclusion: `git stash` is a useful tool for quickly creating a clean working directory.


## Ammending a commit: git commit --amend

- Now say you want to commit your changes, we can do this with `git add src/main.py` followed by `git commit -m "my commit message"`. 
- After commiting you realize actually there was a typo, you should have added `print("Test")`,
not `print("test")`! Making a new commit with `git commit -m "typo"` would create a messy git history.
Instead you can _amend_ the previous commit!
  1. Make the change you want to include in the previous commit: change `print("test")` to `print("Test")`.
  2. Add the changes with `git add src/main.py`.
  3. Amend the previous commit by running `git commit --amend`. A text editor will open where you can
  optionally change the commit message. After exiting the text editor your commit will have been changed.
  4. You can run `git log` to see that you have only added one commit instead of two.


## Interactive rebase: git rebase -i

N.B you can also rebase one branch onto another branch with the `--onto` option.

## Some other useful commands

- [git cherry-pick <commit_hash>](https://git-scm.com/docs/git-cherry-pick): Apply the changes made by the specified commit to your current branch.
- [git restore <path_to_file>](https://git-scm.com/docs/git-restore): Undo all changes to the specified file and restore it 
to the state it was after the latest commit.
- [git reset HEAD~](https://stackoverflow.com/questions/927358/how-do-i-undo-the-most-recent-local-commits-in-git):
Undo a local commit.

