# get list of fonts

Go to the Windows fonts folder and run

```
ls | Select-Object -Property Name | Export-Csv -Path .\fonts.csv
```