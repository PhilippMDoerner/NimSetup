---
tags: github, nim, project, setup
---
# Prepare and publish a nim library on github
## Step 0: Ensure you have doc-comments all over your public-api
This is important in order to generate good reference documentation for your users!

[[Runnable-Examples]] are useful here as well as a demonstration of your API that you know works because those get compiled.

[[Nim Doc Linking]]  to other procs/symbols that you have when your doc comments mention them is also recommended.
## Step 1: Set up nimble doc-generation task
This nimble task compiles your reference documentation based on your doc comments.
Here an example:
```
task docs, "Write the package docs":
	exec "nim doc --verbosity:0 --project --index:on " &
		"--git.url:<Your Project's git url>" &
		"--git.commit:master " &
		"-o:<Folder Where the API-docs shall be stored> " &
		"src/<Core Module of your library>" 
```

## Step 2 (Optional): Set up introductory documentation for newcomers
Docs generated from doc-comments in your source code are highly useful, but mostly as reference documentation.

Ideally you should also provide docs that have sections for:
- QuickStart Examples (Learning/Problem Oriented Documentation)
- Explaining Higher Level Concepts/Architecture ideas and abilities (Understanding Oriented Documentation)
- Showcasing (maybe hidden) Features of your project with examples (Learning/Problem Oriented Documentation) 

Write those docs in different sections in e.g. a [nimibook](https://github.com/pietroppeter/nimibook) . 
Nimibooks are useful here, because they compile and run the code examples you provide in them, giving you guarantees that the code inside of them works.

## Step 3 (Optional): Set up test-calling task 
Only needed if you have complex test setups that e.g. involve containers or testing a GUI, otherwise just use `nimble test`.
Can't give general advice here, as that is highly project specific.

## Step 4: Set up a github test workflow
Create a  .github folder and add the following workflows:
- tests.yml to automatically execute tests when pushing to your develop branch:
```
name: Run Tests

# Execute this workflow only for PRs/Pushes to your develop branch
on:
  push:
    branches:
      - develop
      - main
  pull_request:
    branches:
      - develop
      - main

#Defines the permissions that the default GITHUB_TOKEN secret has that gets generated (see: https://docs.github.com/en/actions/security-guides/automatic-token-authentication)
permissions: 
  contents: write
  id-token: write
  
# Execute a job called "Tests" once for each combination of defined nim-versions and os's.
# It will execute on ubuntu-latest, window-latest and macOs-latest.
# For execution it will install the package according to the nimble file
# and then run the nimble command `test` that executes the tests 
jobs:
  Tests:
    strategy:
      matrix:
        nimversion: 
          - binary:2.0.0
          - binary:1.6.10
        os:
          - ubuntu-latest
          - windows-latest
          - macOS-latest
    runs-on: ${{ matrix.os }}
    timeout-minutes: 30

    name: Nim ${{ matrix.nimversion }} - ${{ matrix.os }}

    steps:
      - uses: actions/checkout@v1
      - uses: iffy/install-nim@v4
        with:
          version: ${{ matrix.nimversion }}
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
      - name: Test
        run: |
          nimble install -y
          nimble test

```

- Dependabot.yml - Dependabot is just general repository utility 
```
# To get started with Dependabot version updates, you'll need to specify which
# package ecosystems to update and where the package manifests are located.
# Please see the documentation for all configuration options:
# https://docs.github.com/github/administering-a-repository/configuration-options-for-dependency-updates

version: 2

updates:

- package-ecosystem: "github-actions" # See documentation for possible values

directory: "/" # Location of package manifests

schedule:

interval: "weekly"
```

## Step 5: Set up a Github doc-compilation workflow
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

# Execute a job called "api-docs" for nim version 1.6.10
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
		run: nimble <doc building command>

	  - name: Copy files to _site directory
		run: |
		  mkdir _site
		  cp -r compiledBook/* _site
	  
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

## Step 6: Publish Package
### Step 6.1: Create Github Token for publishing
- Go to Settings / DeveloperSettings / Personal Access Tokens / Tokens (classic)
- Generate token with access to repository to change it
- Copy the generated token into a `~/.nimble/github_api_token` file
    - This way nimble can use this token to perform the necessary operations for publishing.
	
### Step 6.2: Publish your package
Run `nimble publish` in your project directory.
This will create a PR in nim's [packages](https://github.com/nim-lang/packages) repository to include your package in the list of nim packages.
These are manually reviewed.
Wait for PR to finish.  This may take up to 24h.

