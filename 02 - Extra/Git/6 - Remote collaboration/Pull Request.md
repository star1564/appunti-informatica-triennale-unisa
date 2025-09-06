---
tags:
  - extra/git
cssclasses:
  - mermaid-center
git-section: Remote collaboration
git-section-order: "6"
git-order: "7"
image: git-pull-request-image.png
---

[[Git Commands.base|↖ Ritorna all'indice ↖]]

---
A **Pull Request (PR)** allows you to propose changes to a repository so the project owner can review and merge them. This is the standard workflow for contributing to open source or shared projects.

## Steps

1. Find a repository:
	- Go to the repository you want to contribute to (e.g., on GitHub)
	- Review the `README.md` for guidelines and project context

2. Fork the repository
	- Click "Fork" to create a copy under your own GitHub account
	- This ensures your changes won't directly affect the original project

3. [[Clone]] your fork and `cd` into it

4. Create a new [[Branch]] (use a descriptive name)

5. Make your changes by editing the code or files locally

6. [[Stage]] and [[Commit]] your changes

7. [[Push#^ce54f8|Push the new branch]] to GitHub

8. Open a pull request:
	- On GitHub, go to your forked repository
	- Select the "Pull requests" tab and "New pull request"
	- Set:
	    - *Base branch* to the `main` of the original repo
	    - *Compare branch* to the `new-branch` from your fork.
	- Add a title and description for the request
	- Click "Create pull request"

9. Review and approval:
	- The repository owner(s) review your code
	- They may request changes or approve

10. Merge
	- Once approved, the PR is merged into the original repository
	- Your contribution is now part of the main project

---
- [Fonte](https://www.youtube.com/watch?v=jRLGobWwA3Y)