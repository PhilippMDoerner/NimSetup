---
tags: nim, docs
---
You can link within your doc comments! 
This helps users traverse your documentation easier!

This links to a symbol in the same module:
``## A single option of a select `Form Field<#FormField>`_ with int values`

This links to a symbol of a different module:
```
##`toFormField<../formService.html#toFormField%2COption[bool]%2Cstring%2Cbool>`_ procs.`
```

The general patterns are 
```
`<My Text><#SymbolNameInSameModule>`_

`<My Text><<RelaitveFilePathFromThisModule>#SymbolNameInModule>`_
```
