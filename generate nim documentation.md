___
tags: [nim](nim.md), [nim-docs](nim-docs.md)
Created: 2023-09-02 - 10:35
Updated: `=this.file.mtime`
___
You can generate nim documentation for your project via a [nimble task](nimble%20task.md).
It uses your projects doc-comments and makes it all searchable.

Here an example:
```
task docs, "Write the package docs":
	exec "nim doc --verbosity:0 --project --index:on " &
		"--git.url:<Your Project's git url>" &
		"--git.commit:master " &
		"-o:<Folder Where the API-docs shall be stored> " &
		"src/<Core Module of your library>" 
```
