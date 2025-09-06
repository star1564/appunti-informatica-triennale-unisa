---
tags:
    - extra/git
cssclasses:
    - mermaid-center
git-section: Remote collaboration
git-section-order: "6"
git-order: "4"
image: git-pull-image.png
---


[[Git Commands.base|↖ Ritorna all'indice ↖]]

---

Automates the [[Fetch|fetching]] of changes from remote and updating the local branch refs.

It does three things:
![[Fetch#^36b077]]
![[Fetch#^c16473]]

-   merges the downloaded commits with the local repository

---

```bash
git pull
```

> Fetch from remote and merge with local.

##### Example

<center><b>PUBLISHED</b></center>

```mermaid
%%{init: {"gitGraph": {"mainBranchName": "main"}, "themeVariables": {"git0": "#e63946"}}}%%
gitGraph
    commit id: "0-56ae499"
    commit id: "1-bcd99f2"
    commit id: "2-c987202"
	commit id: "3-be7ac15" tag: "main"
```

<center style="margin-top: 2em"><b>LOCAL</b></center>

```mermaid
gitGraph
	commit id: "0-56ae499"
	commit id: "1-bcd99f2" tag: "o/main" tag: "main*"
```

```bash
$ git pull
```

<center style="margin-bottom: 2em"><b>PUBLISHED</b></center>

```mermaid
%%{init: {"gitGraph": {"mainBranchName": "main"}, "themeVariables": {"git0": "#e63946"}}}%%
gitGraph
    commit id: "0-56ae499"
    commit id: "1-bcd99f2"
    commit id: "2-c987202"
	commit id: "3-be7ac15" tag: "main"
```

<center style="margin-top: 2em; margin-bottom: 2em"><b>LOCAL</b></center>

```mermaid
gitGraph
	commit id: "0-56ae499"
	commit id: "1-bcd99f2"
	commit id: "2-c987202"
	commit id: "3-be7ac15" tag: "main*" tag: "o/main"
```
