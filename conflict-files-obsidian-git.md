---
title: Conflicts
aliases:
  - Conflicts
tags:
created: 2024-08-29 18:27:35
modified: 2024-08-29 18:27:44
---
# Conflicts
Please resolve them and commit them using the commands `Git: Commit all changes` followed by `Git: Push`
(This file will automatically be deleted before commit)
[[#Additional Instructions]] available below file list

- [[README]]

# Additional Instructions
I strongly recommend to use "Source mode" for viewing the conflicted files. For simple conflicts, in each file listed above replace every occurrence of the following text blocks with the desired text.

```diff
<<<<<<< HEAD
    File changes in local repository
=======
    File changes in remote repository
>>>>>>> origin/main
```