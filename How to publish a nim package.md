___
tags: [nim](nim.md)
Created: 2023-09-02 - 11:10
Updated: `=this.file.mtime`
___
Nim packages are made available via a [centralized package list on github](https://github.com/nim-lang/packages/) .
To publish your own package means to have that package added to this list with the appropriate metadata so that it can be downloaded via `nimble install <packageName>`.

You can do so automatically via `nimble`:

### 1. Create Github Token for publishing
- On Github, go to Settings / DeveloperSettings / Personal Access Tokens / Tokens (classic)
- Generate token with access to repository to change it
- Copy the generated token into a `~/.nimble/github_api_token` file
    - This way nimble can use this token to perform as your user on github and create the necessary pull request. 

### 2.Publish your package
Run `nimble publish` in your project directory.
This will create the necessary pull request.
Now you just have to wait for the PR to be accepted, which is a manual process.
This may usually take up to 24h.