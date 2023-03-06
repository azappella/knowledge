# system & hardware time

## System time

You can use date to set the system date. The GNU implementation of date (as found on most non-embedded Linux-based systems) accepts many different formats to set the time, here a few examples:

set only the year:
```
date -s 'next year'
date -s 'last year'
```

set only the month:
```
date -s 'last month'
date -s 'next month'
```

set only the day:
```
date -s 'next day'
date -s 'tomorrow'
date -s 'last day'
date -s 'yesterday'
date -s 'friday'
```
set all together:
```
date -s '2009-02-13 11:31:30' #that's a magical timestamp
```
## Hardware time

Now the system time is set, but you may want to sync it with the hardware clock:

Use --show to print the hardware time:
```
hwclock --show
```
You can set the hardware clock to the current system time:
```
hwclock --hctosys
```
Or the system time to the hardware clock
```
hwclock --systohc
```
