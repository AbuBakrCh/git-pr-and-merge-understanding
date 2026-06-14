## IMPORTANT FOR UNDERSTANDING why sometimes github shows differences when PR is already merged.
- PR commits are the commits reachable from the feature branch that are not reachable from the target branch (main).
- PR file changes are computed by finding the newest common ancestor (merge base) of feature and main, and comparing that commit's snapshot with the feature branch's HEAD commit.

### Rebase
- Git takes the commits that exist on the current branch but not on the target branch, and replays (recreates) them on top of the target branch. On merge, it will be simple fast forward merge.

main
A -> B -> X

feature
A -> B -> C -> D -> E -> F

Running:

git checkout feature
git rebase main

Git identifies that C, D, E, and F are unique to feature, then recreates them on top of X:

A -> B -> X -> C' -> D' -> E' -> F'


### Squash and merge:

- Squash combines multiple commits into one commit.
- Merge integrates changes from one branch into another.

They are different operations, although many Git hosting platforms (e.g., GitHub) offer a "Squash and Merge" button that performs both concepts together:

- Squash all feature branch commits into a single commit.
- Merge that single commit into the target branch.

So:

- Squash ≠ Merge
- Squash and Merge = squash first, then merge the result.
