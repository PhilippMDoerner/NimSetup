___
tags: [github](github.md), [nim](nim.md)
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
See: [generate nim documentation](generate%20nim%20documentation.md)

## Step 3 (Optional): Set up introductory documentation for newcomers
Docs generated from doc-comments in your source code are highly useful, but mostly as reference documentation.

Refer to [documentation](documentation.md) for types of documentation.
Consider [nimibook](nimibook.md) to write documentation with examples that can compile.

## Step 4 (Optional): Set up test-calling task 
Only needed if you have complex test setups that e.g. involve containers or testing a GUI, otherwise just use `nimble test`.
Can't give general advice here, as that is highly project specific.

## Step 5: Set up a github test workflow
Create a  .github folder and add the following workflows:
- [github-test-workflow](github-test-workflow.md)
- [dependabot-workflow](dependabot-workflow.md)
- [github-doc-workflow](github-doc-workflow.md)

## Step 6: Publish Package
In order to make it findable in the [nimble directory](https://nimble.directory) and make it so users can easily install your package via package managers, you need to publish your packages.

Follow the guidelines provided by [How to publish a nim package](How%20to%20publish%20a%20nim%20package.md)