# Getting last modified software on Windows using PowerShell
Getting the list of the last modified software on Windows is actually simple, if using PowerShell

XXXXX

I like getting weird requests and resolving them with one line of code in Powershell. Powershell is indeed a very polished tool for working in the terminal in Windows.

This week I was asked by a friend this question: "hey, how can I get the lastly installed software on my machine"?


## Get-WmiObject
# getting windows diag information - expot all installed software on my machine
```PowerShell
Get-WmiObject -Class Win32_Product | Export-Csv installed.csv
```

And how do I look for what was modified most recently in our machine?

## Get-ChildItem
getting last modified files
```PowerShell
Get-ChildItem C:\ -rec | sort LastWriteTime | select -last 1000 | Export-Csv files.csv
```

Fun! I really don't miss the old DOS command prompt.

# See Also
    * Determining installed .NET Framework versions using PowerShell
    * PowerShell - The server committed a protocol violation
    * Windows Subsystem for Linux, the best way to learn Linux on Windows
    * Why I use Fedora
    * How I fell in love with i3
    * Diagnosing and Fixing WSL initialization issues
    * Creating a Ubuntu Desktop instance on Azure

# References
    * Find latest modified file information in PowerShell
    * Windows : How to list files recursively with size and last access date?
