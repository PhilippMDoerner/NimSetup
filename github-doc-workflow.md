---
Created: 2023-09-02T11:06
Updated: 2023-09-13T19:48
---
___
hubs: [nim](nim.md), [nim-docs](nim-docs.md)
Created: 2023-09-02 - 11:06
Updated: `=this.file.mtime`
___
A github workflow for automatically compiling your project's documentation.
The idea is to [compile your docs](generate%20nim%20documentation%20) via a [nimble task](nimble%20task.md) into a folder that can then be copied into the `_site` folder.

That `_site` folder is then deployed to github pages.
Below is a template for a workflow, placeholders are marked with `<>`.

```
name: github pages

# Execute this workflow only for Pushes to your main branch, not for PRs
on:
  push:
	branches:
	  - main

# Provides the implicitly used and hidden GITHUB_TOKEN the necessary permissions to deploy github_pages
permissions:
  contents: write
  pages: write
  id-token: write

# Execute a job called "api-docs" for nim version 2.0.0
jobs:
  api-docs:
	runs-on: ubuntu-latest
	steps:
	  - uses: actions/checkout@v3
	  - uses: jiro4989/setup-nim-action@v1
		with:
		  nim-version: '1.6.10'

	  - run: nimble install -Y
	  
	  # Optional, if you require any packages to be installed from the package manager
	  # Remove this step if you don't need it.
	  - name: Setup dependencies
		run: |
		  sudo apt update -y
		  sudo apt install -y <space separated package list>
	  
	  
	  - name: Build your docs
		run: nimble <doc building command that outputs to "docFolder">

	  - name: Copy files to _site directory
		run: |
		  mkdir _site
		  cp -r <"docFolder">/* _site
	  
	  - name: Upload  _site directory for deploy job
		uses: actions/upload-pages-artifact@v1 # This will automatically upload an artifact from the '/_site' directory
   
  
  # Deploy _site directory with permissions of GITHUB_TOKEN
  deploy:
	environment:
	  name: github-pages
	runs-on: ubuntu-latest
	needs: api-docs
	steps:
	  - name: Deploy to GitHub Pages
		id: deployment
		uses: actions/deploy-pages@v1
```