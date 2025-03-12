# git-commands

We have already seen that git commits are just snapshots of an entire repository
at one point in time. In this hands on session we are going to explore some useful
git commands for helping keep a modular commit history.

There are two open PRs to show the different branches which we will be using in this
demo. You can see the changes and commits in each branch by checking the PRs.

## Stashing local changes: git stash

- Start by cloning this repository
- Run `git status` to see you are on the main branch with no local changes.
- Make a change in `src/main.py`. Do this by adding `print("test")` at the end of the main function.
- Run `git status` to see that `src/main.py` shows as changed.
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
- After commiting you realize that you forgot to use capital letters, you should have added `print("Test")`,
not `print("test")`! Making a new commit with `git commit -m "typo"` would create a messy git history.
Instead you can _amend_ the previous commit
  1. Make the change you want to include in the previous commit: change `print("test")` to `print("Test")`.
  2. Add the changes with `git add src/main.py`.
  3. Amend the previous commit by running `git commit --amend`. A text editor will open where you can
  optionally change the commit message. After exiting the text editor your commit will have been changed.
  4. You can run `git log` to see that you have only added one commit instead of two.


## Interactive rebase: git rebase -i

Rebase reapplies all the commits in one branch on top of another. It also allows you to edit
the commits as they are being reapplied. Here by rebasing onto main to reapply the 
commits that were made in the `rebase-demo` branch but edit them along the way.

- Look at `rebase-demo` branch. There are five commits, but we made a typo in commit 2. We would like
to fix the typo in commit two and combine commits three, four and five into one commit.
- Begin by switching to the `rebase-demo` branch: `git switch rebase-demo`.
- Now we start the rebase process onto the main branch by running: `git rebase -i main`.
- A text editor will appear with the five commits that are going to be reapplied.
    - Change `pick` to `edit` for the second commit, this will allow us to edit this commit.
    - For commits four and five change `pick` to `squash` this will combine them with commit three.
    - Your text editor should look something like the below image. When it does, save and exit the text editor.
      <img width="413" alt="Screenshot 2025-03-11 at 6 51 41 PM" src="https://github.com/user-attachments/assets/fdeccfcc-d378-48db-bbf6-a4982569cdf6" />

- Now using what we have learned and by running `git status` try to complete the rebase. You should:
  - First fix the typo in commit two (change `print("Commit Twwo")` to `print("Commit Two")`).
  - Then continue the rebase to squash together the commits three, four and five.
- When you have finished run `git log` to see the results. The output should look something like
the below image with commits 3, 4and 5 squashed into one.

  <img width="581" alt="Screenshot 2025-03-11 at 6 53 17 PM" src="https://github.com/user-attachments/assets/e3da4832-b45b-4e5d-8072-d70f4bbf5159" />
  
  If you open `src/main.py` you should see that the typo in commit two is fixed.
  <img width="431" alt="Screenshot 2025-03-11 at 6 53 35 PM" src="https://github.com/user-attachments/assets/c6d6b740-c293-4927-abe8-839dfd07cadc" />




Note: there are alot of other options like deleting commits or swapping commit order that can
be done by rebasing.

Note: If you rebase after pushing your code the commits in your local repo will be out of sync
with the commits on the remote github repo. You can force push to override the changes on github
but this is dangerous and could break other people's code if anybody else is working with the same
commits.

## Some other useful commands

- [git cherry-pick <commit_hash>](https://git-scm.com/docs/git-cherry-pick): Apply the changes made by the specified commit to your current branch.
- [git restore <path_to_file>](https://git-scm.com/docs/git-restore): Undo all changes to the specified file and restore it 
to the state it was after the latest commit.
- [git reset HEAD~](https://stackoverflow.com/questions/927358/how-do-i-undo-the-most-recent-local-commits-in-git):
Undo a local commit.

