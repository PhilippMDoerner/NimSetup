___
tags: [nim](nim.md), [documentation](documentation.md), [nim-docs](nim-docs.md)
Updated: `=this.file.mtime`
Created :  2023-09-02 - 10:26
___
[](https://nim-lang.org/docs/system.html#runnableExamples%2Cstring%2Cuntyped)

Example: 
```
proc double*(a: int): int =
  ## doubles a number
  runnableExamples:
    echo "I got ran!"
    assert 10 == 5.double()
    
  return 2*a
```

RunnableExamples get executed when compiled with `nim doc <project>.nim` or `nimble doc <project>.nim`.

Note: They only get executed on *public procs*. 