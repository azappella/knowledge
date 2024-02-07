# commit types

```
feat – a new feature is introduced with the changes
fix – a bug fix has occurred
chore – changes that do not relate to a fix or feature and don't modify src or test files (for example updating dependencies)
refactor – refactored code that neither fixes a bug nor adds a feature
docs – updates to documentation such as a the README or other markdown files
style – changes that do not affect the meaning of the code, likely related to code formatting such as white-space, missing semi-colons, and so on.
test – including new or correcting previous tests
perf – performance improvements
ci – continuous integration related
build – changes that affect the build system or external dependencies
revert – reverts a previous commit 
```

## long commit message example

```
fix: fix foo to enable bar

This fixes the broken behavior of the component by doing xyz.

BREAKING CHANGE
Before this fix foo wasn't enabled at all, behavior changes from <old> to <new>

Closes D2IQ-12345
```

## other short examples

```
Good

    feat: improve performance with lazy load implementation for images
    chore: update npm dependency to latest version
    Fix bug preventing users from submitting the subscribe form
    Update incorrect client phone number within footer body per client request

Bad

    fixed bug on landing page
    Changed style
    oops
    I think I fixed it this time?
    empty commit messages

```

## References

- https://www.freecodecamp.org/news/how-to-write-better-git-commit-messages/