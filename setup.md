---
Type: readme
---

2 local folders (macbook) for 'coding-practice':

### 1. Notes & script files:
Location: /Users/aksharas/2.Projects/*obsidian_ml_projects/000_coding-practice*

#### Git updating guidelines: 
- Parent folder 'obsidian_ml_projects' is synced to the github repo 'https://github.com/aksharasoman/notes_ml_projects.git' & will be reflected with updates made from the corresponding obsidian vault. 
- Child folder: '000_coding-practice'
	- I can keep the script files in appropriate subfolders inside this child folder.
	- This child folder is mapped to another repository: 'https://github.com/aksharasoman/coding-practice.git'
	- Regular push/pull changes are reflected only in the parent repository.
	- When I want to update the child repository, run the following command:
	- `git subtree push --prefix 000_coding-practice https://github.com/aksharasoman/coding-practice.git main`
		git subtree pull --prefix 000_coding-practice https://github.com/aksharasoman/coding-practice.git main --squash
	
- For more details: [Git Subtree basics · GitHub](https://gist.github.com/SKempin/b7857a6ff6bddb05717cc17a44091202)
	
### 2. webpage files: 
Location: /Users/aksharas/2.Projects/webpage_github/quartoFiles/posts/coding-practice
- Copied files required to update in the webpage to this folder: readme.md as index.qmd & questions_list.qmd 
- 'questions_list.qmd' needs to be regularly updated as 'questions_list.md' file gets updated in the script folder.

### SOP


