# bash shell script to use getopts with gnu style long positional parameters

- October 29th, 2009

Problem: You want to support –really-long-option-names in your bash script and getopts won’t do it.

More problems: You don’t want to use getopt because you want cross platform support or your platform isn’t supported.

More problems than that: I can’t help with existential issues.

Solution: Rewrite the long options into short options and then use getopts.

./args.sh -b -c -a “yada yada” –some-other-gnu –long-gnu-option

content of args.sh:
```
#!/bin/bash
for arg
do
    delim=""
    case "$arg" in
    #translate --gnu-long-options to -g (short options)
        --some-other-gnu) args="${args}-g ";;
       --long-gnu-option) args="${args}-l ";;
       #pass through anything else
       *) [[ "${arg:0:1}" == "-" ]] || delim="\""
           args="${args}${delim}${arg}${delim} ";;
    esac
done

#Reset the positional parameters to the short options
eval set -- $args

while getopts ":a:gl" option 2&gt;/dev/null
do
    case $option in
        g) echo "some other gnu option";;
        l) echo "long gnu option";;
        a) echo ${OPTARG[@]};;
        *) echo $OPTARG is an unrecognized option;;
    esac
done
```

Output:

b is an unrecognized option
c is an unrecognized option
yada yada
some other gnu option
long gnu option

Tags: bash shell getopts positional parameters eval
