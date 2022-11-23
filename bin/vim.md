# vim

## save a file when you forget to sudo vim

```vim
:w !sudo tee %
```

## replace dos line endings with unix ones

```vim
:%s/^M//g
```

```vim
:set fileformat=unix
:w
```