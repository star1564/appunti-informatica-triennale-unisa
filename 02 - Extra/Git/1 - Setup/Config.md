---
tags:
  - extra/git
git-section: Setup
git-section-order: "1"
git-order: "1"
image: git-config-image.png
---

[[Git Commands.base|↖ Ritorna all'indice ↖]]

---

Configure user information used across all local repositories.

## Set important information about the user

```bash
git config --global user.name "[firstname lastname]"
```

 >Set a name that is identifiable for credit when review version history. Avoid using a nickname.


```bash
git config --global user.email "[valid-email]"
``` 

 >Set an email address that will be associated with each history marker. 
 

## In case it is not already set

```bash
git config --global color.ui auto 
```

>Set automatic command line coloring for Git for easy reviewing.
