```
*    *    *    *    *  command to be executed
┬    ┬    ┬    ┬    ┬
│    │    │    │    │
│    │    │    │    │
│    │    │    │    └───── day of week (0 - 6) (0 is Sunday, or use names)
│    │    │    └────────── month (1 - 12)
│    │    └─────────────── day of month (1 - 31)
│    └──────────────────── hour (0 - 23)
└───────────────────────── min (0 - 59)
```

To open crontab

    crontab -e

If you need to run commands with administrative privileges

    sudo crontab -e

To list crontab content

    crontab -l

To remove all your cron jobs

    crontab -r