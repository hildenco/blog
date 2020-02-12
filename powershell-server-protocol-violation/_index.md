#  PowerShell - The server committed a protocol violation
On this post, let's understand what that error means and review 3 different ways to fix it.

<div class="subheader">Photo by Max Rovensky on <a href="https://unsplash.com/photos/qEEQ5wkggCE">Unsplash</a><br /></div>

I was trying to write a simple crawler to validate some resources against some URLs on a page with PowerShell and was got the error:

The server committed a protocol violation

After googling ducking around, I found three different solutions for this problem:

    Modifying your powershell.exe.config;
    Modifying your request in order to avoid it stopping if your url is invalid in .NET
    Modifying your request in order to avoid it stopping if your url is invalid in PowerShell.

Let's review each of them and understand a little more about the problem. 

## Solution 1 - Modifying powershell.exe.config
This solution will apply to all PowerShell requests so use with caution. Also, this solution is not portable so, if you plan to run this script on a machine you don't have admin access to, this solution will probably not work.
```cs
<system.net>
 <settings>
  <httpWebRequest useUnsafeHeaderParsing="true" />
 </settings>
</system.net>
```

## Solution 2 - Modifying your request in .NET
To modify your request, have the code below before you actually run Invoke-WebRequest:

```PowerShell
$netAssembly = [Reflection.Assembly]::GetAssembly([System.Net.Configuration.SettingsSection])
if($netAssembly)
{
    $bindingFlags = [Reflection.BindingFlags] "Static,GetProperty,NonPublic"
    $settingsType = $netAssembly.GetType("System.Net.Configuration.SettingsSectionInternal")

    $instance = $settingsType.InvokeMember("Section", $bindingFlags, $null, $null, @())

    if($instance)
    {
        $bindingFlags = "NonPublic","Instance"
        $useUnsafeHeaderParsingField = $settingsType.GetField("useUnsafeHeaderParsing", $bindingFlags)

        if($useUnsafeHeaderParsingField)
        {
          $useUnsafeHeaderParsingField.SetValue($instance, $true)
        }
    }
}
# Powershell - The server committed a protocol violation.ps1 hosted with ❤ by GitHub
```

## Solution 3 - Modifying your request in PowerShell
This is probably the simplest. Just run the line below:
[System.Net.ServicePointManager]::ServerCertificateValidationCallback = { $true }
See Also

    Getting last modified software on Windows using PowerShell
    Determining installed .NET Framework versions using PowerShell
    Windows Subsystem for Linux, the best way to learn Linux on Windows
    Why I use Fedora Linux
    How I fell in love with i3
    Diagnosing and Fixing WSL initialization issues
    Creating a Ubuntu Desktop instance on Azure

References

    Invoke-WebRequest
    Ignoring SSL trust in PowerShell System.Net.WebClient
    Simple Invoke-WebRequest produces protocol violation when attempting to XML login to a web based peripheral


