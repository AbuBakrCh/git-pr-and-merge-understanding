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
Rebase → rewrites the feature branch's commit history.
Squash and Merge → leaves the feature branch history alone, but creates a new squashed commit on the target branch


<img width="977" height="295" alt="image" src="https://github.com/user-attachments/assets/fb5f4fd4-3fe7-40b7-82f3-e64ab845c9b2" />
