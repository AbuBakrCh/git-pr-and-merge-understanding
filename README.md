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


### Mental model
Rebase → rewrites the feature branch's commit history by recreating its commits on top of the latest commit from the target branch.

Rebase and Merge → leaves the feature branch unchanged, but GitHub/GitLab takes the commits from the feature branch, replays them onto the target branch, and fast-forwards the target branch (no merge commit).

Squash and Merge → leaves the feature branch unchanged, but creates a new squashed commit on the target branch.

Merge (Create Merge Commit) → leaves the feature branch unchanged and creates a merge commit on the target branch.
