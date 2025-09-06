---
tags:
    - extra/git
cssclasses:
    - mermaid-center
git-section: Remote collaboration
git-section-order: "6"
git-order: "5"
image: git-push-image.png
---

[[Git Commands.base|↖ Ritorna all'indice ↖]]

---
This command is responsible for uploading _your_ changes to a specified remote and updating that remote to incorporate your new commits.

---

```bash
git push
```

> Update remote refs along with associated objects.



> [!TIP] Pushing a branch that doesn't exist in remote
> ```bash
> git push --set-upstream origin [local-only-branch]
> ```

^ce54f8


##### Example

<center style="margin-bottom: 2em"><b>PUBLISHED</b></center>

```mermaid
%%{init: {"gitGraph": {"mainBranchName": "main"}, "themeVariables": {"git0": "#e63946"}}}%%
gitGraph
    commit id: "0-56ae499"
    commit id: "1-bcd99f2" tag: "main"
```

<center style="margin-top: 2em; margin-bottom: 2em"><b>LOCAL</b></center>

```mermaid
gitGraph
	commit id: "0-56ae499"
	commit id: "1-bcd99f2" tag: "o/main"
	commit id: "2-c987202" tag: "main*"
```

```bash
$ git push
```

<center style="margin-bottom: 2em"><b>PUBLISHED</b></center>

```mermaid
%%{init: {"gitGraph": {"mainBranchName": "main"}, "themeVariables": {"git0": "#e63946"}}}%%
gitGraph
    commit id: "0-56ae499"
    commit id: "1-bcd99f2"
    commit id: "2-c987202" tag: "main"
```

<center style="margin-top: 2em; margin-bottom: 2em"><b>LOCAL</b></center>

```mermaid
gitGraph
	commit id: "0-56ae499"
	commit id: "1-bcd99f2"
	commit id: "2-c987202" tag: "o/main" tag: "main*"
```
