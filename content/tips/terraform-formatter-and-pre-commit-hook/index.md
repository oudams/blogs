---
title: "Terraform Formatter and Pre Commit Hook"
tips: ["formatter", "lint"]
tags: ["terraform", "tips"]
date: 2022-03-24T06:38:57+08:00
draft: false
---

Our goal is to lay out and prioritise areas to improve.

# Context
### Basic Practices
1. **Formatting**:
    It is crucial in software development team to have a formatting practice, and most importantly the techniques and tools are agreed upon. This benefits us in terms of development productivity. For example ... (Consider merge conflicts, consistency and readability)
2. **Static Analysis**:
   In Software development practice, we may order the testing approaches as:
    ```shell
    static analysis > unit test > integration > end-to-end > ...
    ```

      Static analysis may "test" our code to catch:
      1. Code Validation: Potential compilation/interpretation error -- such as typo, undeclared function/var, syntax error
      2. Terraform security and compliance

### About pre-commit Hook
Certain error can be catch after editing but **before "git commit"**. To use it, it has to be installed in Dev machine, but important and good that the configuration can be shared across time.

Refer to the **[pre-commit hook framework](https://pre-commit.com/)** for more details and advance usage.


# What Tools & When
There are independent open source tools we can use to integrate with terraform development workflows. In practice, we may use pre-commit hook for certain tools(used for Static Analysis).

1. Auto Formatting
   - Why
     - We want to team to share the same techniques and tool when it come to formatting. We can increase productivity by reducing conflicts. 
   - When
       - Code editing
   - Tools:
       - `terraform fmt`
       - `intelliJ File Watcher` plugin

2. Static Analysis:
   1. Validation
      - Why
        - Usually some error is caught during `terraform plan`, but we don't enforce team to run `terraform plan` every time they `git commit/push`, meaning we have to wait till our code delivered to the specific pipeline then get informed about the error. So the balance is to enforce a short and sweet `terraform validate` execution in pre-commit hook, we can catch those error that `plan` would catch. Now we shorten the feedback leap time, and the respond is pretty fast.
      - Tools:
        - `terraform validate`
      - When:
        - pre-commit hook
        - Pre-package step **pipeline**
   2. Checkov
       - **Note**: To really work with this  as we need a baseline configs that works for our cases?**
       - [] Can it work with terraform plan or tfvars?
       - [] What are the baseline configs that work in terms of *Terraform security and compliance*
   3. Other tool to consider:
      1. `TFLint`
      2. `TFSec`
        the questions are
         - **Compatibility: are we ready to use these as we need a baseline configs that works for our cases?**
         - Compatibility: smart enough to interpret tfvars or can work with `terraform plan` file?
         - Functionality: is it just a duplication of `checkov`?

# How
## Formatting

https://www.jetbrains.com/help/idea/using-file-watchers.html

## Terrform pre-commit

https://github.com/antonbabenko/pre-commit-terraform