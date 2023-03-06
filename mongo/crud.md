# crud

## insert

```js
db.collectionName.insertOne
db.collectionName.insertMany
```

```js
db.movies.insertOne({"title" : "Stand by Me"})
```

insertOne will add an `"_id"` key to the document (if you do not supply one) and store the document in MongoDB.  

```js

db.movies.insertMany([
	{"title" : "Ghostbusters"},
 	{"title" : "E.T."},
 	{"title" : "Blade Runner"}]
);
```

insertMany is useful if you are inserting multiple documents into a single collection. If you are just importing raw data (e.g., from a data feed or MySQL), there are command-line tools like mongoimport that can be used instead of a batch insert.

### ordered inserts

When performing a bulk insert using insertMany, if a document halfway through the array produces an error of some type, what happens depends on whether you have opted for ordered or unordered operations. As the second parameter to insertMany you may specify an options document. Specify true for the key "ordered" in the options document to ensure documents are inserted in the order they are provided. Specify false and MongoDB may reorder the inserts to increase performance. Ordered inserts is the default if no ordering is specified. For ordered inserts, the array passed to insertMany defines the insertion order. If a document produces an inser‐ tion error, no documents beyond that point in the array will be inserted. For unor‐ dered inserts, MongoDB will attempt to insert all documents, regardless of whether some insertions produce errors.


```js
> db.movies.insertMany([...], {"ordered" : false})
```


## update

```
db.collectionName.update
db.collectionName.insertMany
```


