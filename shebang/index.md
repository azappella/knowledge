# shebang

The #! syntax used in scripts to indicate an interpreter for execution under UNIX / Linux operating systems. Most Linux shell and perl / python script starts with the following line:

```
#!/bin/bash
```

## bash shebang

```
#!/bin/bash
```

## node shebang

```
#!/usr/bin/env node
```

## perl shebang

#!/usr/bin/perl

## python2 shebang

#!/usr/bin/python

## python3 shebang

#!/usr/bin/python3

## portable shebang

The /usr/bin/env runs a program such as a bash in a modified environment. It makes your bash script portable.
The advantage of #!/usr/bin/env bash is that it will use whatever bash executable appears first in the running user's $PATH variable.

```
#!/usr/bin/env bash
```




