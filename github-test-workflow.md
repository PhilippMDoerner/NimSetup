___
tags: [github](github.md), [maintenance](maintenance.md), [nim](nim.md)
Created: 2023-09-02 - 10:58
Updated: `=this.file.mtime`
___
The following is an example of a `tests.yml` file on how you can set up a workflow to execute tests for a [nim](nim.md)-project in github.

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