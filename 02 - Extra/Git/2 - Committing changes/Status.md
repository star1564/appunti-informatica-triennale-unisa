---
tags:
  - extra/git
cssclasses:
  - mermaid-center
git-section: Committing changes
git-section-order: "2"
git-order: "1"
image: git-status-image.png
---

[[Git Commands.base|↖ Ritorna all'indice ↖]]

---
The **status** will display the two types of files:
- *Tracked*: these are files that have been *added to the repository* at some point.
- *Untracked*: these are *new files in your working directory*, so Git ignores the edits applied to them (until they're added to the [[Stage]]).

## Get repository status

```bash
git status
```

 >Show modified files in working directory, [[Stage|staged]] for your next commit.