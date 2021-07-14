# inotifywait

--> Check fswatch (which actually wraps inotifywait on most distributions)

## Simple usage
```
inotifywait -m -e modify /tmp/testfile
```

## Events

```
access          file or directory contents were read
modify          file or directory contents were written
attrib          file or directory attributes changed
close_write     file or directory closed, after being opened in
                writable mode
close_nowrite   file or directory closed, after being opened in
                read-only mode
close           file or directory closed, regardless of read/write mode
open            file or directory opened
moved_to        file or directory moved to watched directory
moved_from      file or directory moved from watched directory
move            file or directory moved to or from watched directory
create          file or directory created within watched directory
delete          file or directory deleted within watched directory
delete_self     file or directory was deleted
unmount         file system containing file or directory unmounted
```

## Example

If some program replaces myfile.py with a different file, rather than writing to the existing myfile, inotifywait will die. Many editors work that way.
To overcome this limitation, use inotifywait on the directory:
```
inotifywait -e close_write,moved_to,create -m . |
while read -r directory events filename; do
  if [ "$filename" = "test.py" ]; then
    echo "I can now rsync"
  fi
done
```