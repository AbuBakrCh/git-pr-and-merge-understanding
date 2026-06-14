## IMPORTANT FOR UNDERSTANDING why sometimes github shows differences when PR is already merged.
- PR commits are the commits reachable from the feature branch that are not reachable from the target branch (main).
- PR file changes are computed by finding the newest common ancestor (merge base) of feature and main, and comparing that commit's snapshot with the feature branch's HEAD commit.


# Rebase vs Rebase and Merge vs Squash and Merge

## Rebase (`git rebase main`)

**Rebase onto main** means running:

```bash
git checkout feature
git rebase main
```

This operation **changes the feature branch** and **does not change `main`**.

### Example

```text
main
A --- B --- S --- X

feature
A --- B --- C --- D --- E --- F
```

Where `S` is the squash commit that contains the changes from `C + D + E`.

When you run:

```bash
git rebase main
```

Git takes the commits that are unique to the feature branch and tries to replay them on top of the latest commit of `main` (`X`). Since the changes from `C`, `D`, and `E` are already present in `main` through `S`, Git will usually skip those commits and reapply only `F`.

### Result

```text
main
A --- B --- S --- X

feature
A --- B --- S --- X --- F'
```

`F'` contains the same changes as `F`, but it is a new commit with a different hash.

### Key Points

<img width="985" height="317" alt="image" src="https://github.com/user-attachments/assets/fac80c94-e995-4fb0-93d2-154bfa17c342" />

* Changes the **feature branch**.
* Does **not** change `main`.
* Replays feature branch commits on top of the latest `main`.
* Commonly used after a squash merge when continuing work on the same branch.

---

## Rebase and Merge (GitHub/GitLab PR Option)

**Rebase and Merge** affects the **target branch (`main`)** and leaves the **feature branch unchanged**.

### Before

```text
main
A --- B

feature
A --- B --- C --- D
```

### After Rebase and Merge

```text
main
A --- B --- C' --- D'

feature
A --- B --- C --- D
```

GitHub/GitLab rebases the feature commits onto the latest `main` and then adds those rebased commits to `main`.

### Key Points

* Changes **main**.
* Does **not** change the feature branch.
* No merge commit is created.
* Maintains a linear history.
* New commit hashes (`C'`, `D'`) are created on `main`.

---

## Squash and Merge (GitHub/GitLab PR Option)

**Squash and Merge** affects the **target branch (`main`)** and leaves the **feature branch unchanged**.

### Before

```text
main
A --- B

feature
A --- B --- C --- D --- E
```

### After Squash and Merge

```text
main
A --- B --- S

feature
A --- B --- C --- D --- E
```

Where `S` is a new commit containing the combined changes from `C + D + E`.

### Key Points

* Changes **main**.
* Does **not** change the feature branch.
* Individual commits (`C`, `D`, `E`) do not appear on `main`.
* A single squashed commit (`S`) appears on `main`.
* Produces a clean, linear history.

---

## Why Rebase After a Squash Merge?

After a squash merge:

```text
main
A --- B --- S

feature
A --- B --- C --- D --- E --- F
```

The feature branch still contains `C`, `D`, and `E`, even though their changes are already present in `main` through `S`.

If you plan to continue working on the same feature branch, it is recommended to update it before opening another PR:

```bash
git checkout feature
git rebase main
```

or

```bash
git checkout feature
git reset --hard main
```

This updates the **feature branch** so that it starts from the latest state of `main` and avoids carrying forward commits that have already been merged.

---

## Summary

| Operation                   | Changes Feature Branch? | Changes Main Branch? | Creates Merge Commit? |
| --------------------------- | ----------------------- | -------------------- | --------------------- |
| `git rebase main`           | ✅ Yes                   | ❌ No                 | ❌ No                  |
| Rebase and Merge            | ❌ No                    | ✅ Yes                | ❌ No                  |
| Squash and Merge            | ❌ No                    | ✅ Yes                | ❌ No                  |
| Merge (Create Merge Commit) | ❌ No                    | ✅ Yes                | ✅ Yes                 |

### Quick Memory Rule

* **`git rebase main`** → rewrites the **feature branch**.
* **Rebase and Merge** → rebases commits and adds them to **main**.
* **Squash and Merge** → creates one new squashed commit on **main**.
* **Merge Commit** → preserves branch history and creates a merge commit on **main**.
