# windows server 2019

After creating a AD DS, the network admin encounters the following:

```
Windows cannot access the specified device, path, or file. You may not have the appropriate permissions to access the item
```

Then run

```
1) run secpol.msc
2) Computer Configuration > Windows Settings > Security Settings > Local Policies > Security Options
3) enable User Account Control: Admin Approval Mode for the Built-in Administrator account
```

source: https://www.reddit.com/r/sysadmin/comments/cb4mfj/windows_server_2019_windows_cannot_access_the/r