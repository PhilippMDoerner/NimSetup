---
Created: 2023-09-02T11:03
Updated: 2023-09-21T17:13
---
___
hubs: [github](github.md), [maintenance](maintenance.md)
Created: 2023-09-02 - 11:03
Updated: `=this.file.mtime`
___
Dependabot is a [feature provided by github](https://docs.github.com/en/code-security/dependabot) that allows monitoring your project and auto-updating the actions you use etc.

The following is an example `dependabot.yml`: 
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