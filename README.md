IMPORTANT FOR UNDERSTANDING why sometimes github shows differences when PR is already merged.
- PR commits are the commits reachable from the feature branch that are not reachable from the target branch (main).
- PR file changes are computed by finding the newest common ancestor (merge base) of feature and main, and comparing that commit's snapshot with the feature branch's HEAD commit.

Rebase
- Rebase moves the branch to the target branch's HEAD and then recreates only the commits that exist on the current branch but not on the target branch.
