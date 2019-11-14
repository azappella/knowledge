# check hardware virtualization support

```
grep --color vmx /proc/cpuinfo ## for an Intel processor
grep --color svm /proc/cpuinfo ## for an AMD processor
```