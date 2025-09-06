---
tags:
    - extra/git
cssclasses:
    - mermaid-center
git-section: Reversing changes
git-section-order: "5"
git-order: "1"
image: git-reset-image.png
---

[[Git Commands.base|↖ Ritorna all'indice ↖]]

---

One of the two primary ways to undo changes in Git.

> [!warning]
> This method does not work for [[Remote Branches]], only for local branches on your machine.

## Reset

```bash
git reset [commit-id-or-ref]
```

> This reverses changes by moving a branch [[Relative Refs#Reassign a branch to a commit|moving a branch reference backwards in time to an older commit]], as if the newer commit had never been made in the first place.

##### Example

```mermaid
gitGraph
	commit id: "0-56ae499"
	commit id: "1-bcd99f2"
	commit id: "2-c987202" tag: "main*"
```

```bash
$ git reset HEAD^
```

```mermaid
gitGraph
	commit id: "0-56ae499"
	commit id: "1-bcd99f2" tag: "main*"
	commit id: "2-c987202 (IGNORED)" type: REVERSE
```

> [!NOTE]+
> Each time you reset a branch to a commit, the changes made by the commits after this one will be ignored.

```bash
$ git commit -m "A new commit"
```

```mermaid
gitGraph
	commit id: "0-56ae499"
	commit id: "1-bcd99f2"
	commit id: "2-c987202 (IGNORED)" type: REVERSE
	commit tag: "main*"
```

### Soft reset

```bash
git reset --soft [commit-id-or-ref]
```

> This command is used to undo the last commit **while keeping the changes in the [[Stage|staging area]]**, allowing you to modify or recommit them later.

> [!TIP]+
> If you reset only one commit, this is useful for when you have forgotten to add something to it (instead of using [[Commit amend]]).

## Keep some changes

Use [[Rebase#Interactive rebase|Interactive rebase]] with the HEAD ref to select what commits to keep

```mermaid
gitGraph
	commit id: "0-56ae499"
	commit id: "1-bcd99f2"
	commit id: "2-80bebfc" type: HIGHLIGHT
	commit id: "3-826be44" type: HIGHLIGHT
	commit id: "4-bdf205a"
	commit id: "5-f7e54ae" tag: "main*"
```

```bash
$ git rebase -i HEAD~4
```

```
TEXT OPENED:
3-826be44 # Put this first
2-80bebfc
```

```mermaid
gitGraph
	commit id: "0-56ae499"
	commit id: "1-bcd99f2"
	branch IGNORED
	commit id: "2-80bebfc (IGNORED)" type: REVERSE
	commit id: "3-826be44 (IGNORED)" type: REVERSE
	commit id: "4-bdf205a (IGNORED)" type: REVERSE
	commit id: "5-f7e54ae (IGNORED)" type: REVERSE
	checkout main
	commit id: "3-826be44" type: HIGHLIGHT
	commit id: "2-80bebfc" type: HIGHLIGHT tag: "main*"
```
