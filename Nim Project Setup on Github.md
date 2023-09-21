---
Created: 13-08-2023 08:12:11 am
Updated: 2023-09-21T17:13
---
___
hubs: [github](github.md), [nim](nim.md)
Updated: `=this.file.mtime`
Created :  2023-09-02 - 10:31
___
Once you have developed your project locally and it works as you want, you may want to push your project to github in order to share it with others.
In order to improve the quality of the package, these are things you can do.

## Step 0: Ensure you have tests
Tests are necessary to be able to build reliability.
Make sure your public-api is tested in order to provide guarantees to the user that feature X will work in a specific way.

## Step 1: Ensure you have doc-comments all over your public-api
This is important in order to generate good reference documentation for your users!

[Runnable examples](Runnable-Examples.md) are useful here as well as a demonstration of your API that you know works because those get compiled.

It is also recommended to [link in doc-comments](Nim%20Doc%20Linking.md) to other procs/symbols that are relevant for a given proc.

## Step 2: Set up nimble doc-generation task
[See here](generate%20nim%20documentation.md) for a guide on how to set one up.

## Step 3 (Optional): Set up introductory documentation for newcomers
Docs generated from doc-comments in your source code are highly useful, but mostly as reference documentation.

You can also write [other kinds of documentation](documentation.md) e.g. to explain the individual features of the package and their architecture better. 

Consider [nimibook](nimibook.md) to write such documentation, as you can write examples similar to [runnable examples](Runnable-Examples.md) that get compiled and have everything in a structure overview.

## Step 4: Enable github pages on your github repository
Go to Settings => Environments look for a, or create a, github pages environment.
Under `Deployment branches` add your main branch.
## Step 5 (Optional): Set up test-calling task 
Only needed if you have complex test setups that e.g. involve containers or testing a GUI, otherwise just use `nimble test`.
Can't give general advice here, as that is highly project specific.

## Step 6: Set up github workflows
To enable automatic compilation + deployment of your docs and automatic execution of your tests, setup github workflows.

Create a  .github folder and add the following workflow-files:
- [github-test-workflow](github-test-workflow.md)
- [dependabot-workflow](dependabot-workflow.md)
- [github-doc-workflow](github-doc-workflow.md)

## Step 7: Publish Package
In order to make it find-able in the [nimble directory](https://nimble.directory) and make it so users can easily install your package via package managers, you need to publish your packages.

Follow the guidelines provided by [How to publish a nim package](How%20to%20publish%20a%20nim%20package.md)