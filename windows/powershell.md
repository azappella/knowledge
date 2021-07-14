# PowerShell

## PowerShell variables
[Documentation](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_profiles?view=powershell-7)

```
$PSHOME
$PROFILE
```

## Environment variables
```
$env:PATH
$env:COMPUTERNAME
$env:APPDATA
$env:ProgramFiles
${env:ProgramFiles(x86)}
$env:USERNAME
$env:USERPROFILE
```

## Discovery & help

```
Get-Command *keyword*

Get-Command *process*

Get-Command *date*
```

```
Get-Help Get-Date
```

## Format output

```
command | Format-Table

command | Format-List
```

## Equality checks

| Type        | Operators    | Description                                  |
| ----------- | ------------ | -------------------------------------------- |
| Equality    | -eq          | equals                                       |
|             | -ne          | not equals                                   |
|             | -gt          | greater than                                 |
|             | -ge          | greater than or equal                        |
|             | -lt          | less than                                    |
|             | -le          | less than or equal                           |
|             |              |                                              |
| Matching    | -like        | Returns true when string matches wildcard    |
|             |              | pattern                                      |
|             | -notlike     | Returns true when string does not match      |
|             |              | wildcard pattern                             |
|             | -match       | Returns true when string matches regex       |
|             |              | pattern; $matches contains matching strings  |
|             | -notmatch    | Returns true when string does not match      |
|             |              | regex pattern; $matches contains matching    |
|             |              | strings                                      |
|             |              |                                              |
| Containment | -contains    | Returns true when reference value contained  |
|             |              | in a collection                              |
|             | -notcontains | Returns true when reference value not        |
|             |              | contained in a collection                    |
|             | -in          | Returns true when test value contained in a  |
|             |              | collection                                   |
|             | -notin       | Returns true when test value not contained   |
|             |              | in a collection                              |
|             |              |                                              |
| Replacement | -replace     | Replaces a string pattern                    |
|             |              |                                              |
| Type        | -is          | Returns true if both object are the same     |
|             |              | type                                         |
|             | -isnot       | Returns true if the objects are not the same |
|             |              | type

### Display an objects members (methods, properties, etc.)

```
Get-Date | Get-Member

```

### Display only properties 

```
Get-Date | Format-List

DisplayHint : DateTime
Date        : 3/27/2020 12:00:00 AM
Day         : 27
DayOfWeek   : Friday
DayOfYear   : 87
Hour        : 10
Kind        : Local
Millisecond : 385
Minute      : 35
Month       : 3
Second      : 50
Ticks       : 637209021503855888
TimeOfDay   : 10:35:50.3855888
Year        : 2020
DateTime    : Friday, March 27, 2020 10:35:50 AM
```

## Working with dates

[Documentation](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/get-date?view=powershell-7)

```
Get-Date
   [[-Date] <DateTime>]
   [-Year <Int32>]
   [-Month <Int32>]
   [-Day <Int32>]
   [-Hour <Int32>]
   [-Minute <Int32>]
   [-Second <Int32>]
   [-Millisecond <Int32>]
   [-DisplayHint <DisplayHintType>]
   [-Format <String>]
   [<CommonParameters>]

```

### Formatting

| Specifier | Definition                                            |
| --------- | ----------------------------------------------------- |
| `dddd`    | Day of the week - full name                           |
| `MM`      | Month number                                          |
| `dd`      | Day of the month - 2 digits                           |
| `yyyy`    | Year in 4-digit format                                |
| `HH:mm`   | Time in 24-hour format -no seconds                    |
| `K`       | Time zone offset from Universal Time Coordinate (UTC) |


### Format current date

```
(Get-Date).ToString('yyyy-MM-dd') 
```

### Get a date minus a number of days

```
(Get-Date).addDays(-7).ToString('yyyy-MM-dd') 
```

### Set a date

```
(Get-Date -year 2020 -month 1 -day 1).toString('yyyy-MM-dd')
```