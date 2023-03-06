# mongo shell


## javascript equivalent to shell helpers

```
use video | db.getSisterDB("video") 
show dbs | db.getMongo().getDBs() 
show collections | db.getCollectionNames()
```

## .mongorc.js

If you have frequently loaded scripts, you might want to put them in your .mongorc.js file. This file is run whenever you start up the shell.

You can disable loading your .mongorc.js by using the --norc option when starting the shell.

## setting up an editor

```sh
EDITOR="/usr/bin/emacs"
```

Now, if you want to edit a variable, you can say edit varname—for example:

```js
> var wap = db.books.findOne({title: "War and Peace"});
> edit wap
```