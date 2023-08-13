---
tags: nim, docs, project, setup
---
[Docs](https://nim-lang.org/docs/system.html#runnableExamples%2Cstring%2Cuntyped)

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