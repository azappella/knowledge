# merge histories

```
mkdir yasikovsky && cd yasikovsky

git fetch git@github.com:yasikovsky/technis-test.git main           

git merge -s ours --no-commit --allow-unrelated-histories FETCH_HEAD

git read-tree --prefix=yasikovsky/ -u FETCH_HEAD 
```