---
external help file: Microsoft.PowerShell.Commands.Utility.dll-Help.xml
keywords: powershell,cmdlet
locale: en-us
Module Name: Microsoft.PowerShell.Utility
ms.date: 06/09/2017
online version: https://go.microsoft.com/fwlink/?linkid=2097040
schema: 2.0.0
title: Write-Information
---

# Write-Information

## SYNOPSIS

Specifies how PowerShell handles information stream data for a command.

## SYNTAX

```
Write-Information [-MessageData] <Object> [[-Tags] <String[]>] [<CommonParameters>]
```

## DESCRIPTION

The `Write-Information` cmdlet specifies how PowerShell handles information stream data for a command.

Windows PowerShell 5.0 introduces a new, structured information stream (number 6 in PowerShell streams) that you can use to transmit structured data between a script and its callers (or hosting environment).
`Write-Information` lets you add an informational message to the stream, and specify how PowerShell handles information stream data for a command. Information streams also work for `PowerShell.Streams`, jobs, scheduled jobs, and workflows.

> [!NOTE]
> The information stream does not follow the standard convention of prefixing its messages with "[Stream Name]:".  This was intended for brevity and visual cleanliness.

The `$InformationPreference` preference variable value determines whether the message you provide to `Write-Information` is displayed at the expected point in a script's operation.
Because the default value of this variable is `SilentlyContinue`, by default, informational messages are not shown.
If you don't want to change the value of `$InformationPreference`, you can override its value by adding the `InformationAction` common parameter to your command.
For more information, see [about_Preference_Variables](../Microsoft.PowerShell.Core/About/about_Preference_Variables.md) and [about_CommonParameters](../Microsoft.PowerShell.Core/About/about_CommonParameters.md).

> [!NOTE]
> Starting in Windows PowerShell 5.0, `Write-Host` is a wrapper for `Write-Information`
> This allows you to use `Write-Host` to emit output to the information stream.
> This enables the **capture** or **suppression** of data written using `Write-Host` while preserving backwards compatibility.
> for more information see [Write-Host](Write-Host.md)

`Write-Information` is also a supported workflow activity.

## EXAMPLES

### Example 1: Write information for Get- results

```powershell
Get-WindowsFeature -Name p*; Write-Information -MessageData "Got your features!" -InformationAction Continue
```

```output
Display Name                                            Name                       Install State
------------                                            ----                       -------------
[ ] Print and Document Services                         Print-Services                 Available
    [ ] Print Server                                    Print-Server                   Available
    [ ] Distributed Scan Server                         Print-Scan-Server              Available
    [ ] Internet Printing                               Print-Internet                 Available
    [ ] LPD Service                                     Print-LPD-Service              Available
[ ] Peer Name Resolution Protocol                       PNRP                           Available
[X] Windows PowerShell                                  PowerShellRoot                 Installed
    [X] Windows PowerShell 5.0                          PowerShell                     Installed
    [ ] Windows PowerShell 2.0 Engine                   PowerShell-V2                    Removed
    [X] Windows PowerShell ISE                          PowerShell-ISE                 Installed
Got your features!
```

In this example, you show an informational message, "Got your features!", after running the `Get-WindowsFeature` command to find all features that have a Name value that starts with 'p'.
Because the `$InformationPreference` variable is still set to its default, `SilentlyContinue`, you add the `InformationAction` parameter to override the `$InformationPreference` value, and show the message.
The `InformationAction` value is Continue, which means that your message is shown, but the script or command continues, if it is not yet finished.

### Example 2: Write information and tag it

```powershell
Get-WindowsFeature -Name p*; Write-Information -MessageData "To filter your results for PowerShell, pipe your results to the Where-Object cmdlet." -Tags "Instructions" -InformationAction Continue
```

```output
Display Name                                            Name                       Install State
------------                                            ----                       -------------
[ ] Print and Document Services                         Print-Services                 Available
    [ ] Print Server                                    Print-Server                   Available
    [ ] Distributed Scan Server                         Print-Scan-Server              Available
    [ ] Internet Printing                               Print-Internet                 Available
    [ ] LPD Service                                     Print-LPD-Service              Available
[ ] Peer Name Resolution Protocol                       PNRP                           Available
[X] Windows PowerShell                                  PowerShellRoot                 Installed
    [X] Windows PowerShell 5.0                          PowerShell                     Installed
    [ ] Windows PowerShell 2.0 Engine                   PowerShell-V2                    Removed
    [X] Windows PowerShell ISE                          PowerShell-ISE                 Installed
To filter your results for PowerShell, pipe your results to the Where-Object cmdlet.
```

In this example, you use `Write-Information` to let users know they'll need to run another command after they're done running the current command.
The example adds the tag Instructions to the informational message.
After running this command, if you search the information stream for messages tagged Instructions, the message specified here would be among the results.

### Example 3: Write information to a file

```powershell
function Test-Info
{
    Get-Process P*
    Write-Information "Here you go"
}
Test-Info 6> Info.txt
```

In this example, you redirect the information stream in the function to a file, Info.txt, by using the code 6\>.
When you open the Info.txt file, you see the text, "Here you go."

### Example 4: Pass object to write information

```powershell
Get-Process | Sort-Object CPU -Descending | Select-Object Id, ProcessName, CPU -First 10 | Write-Information -InformationAction Continue
```

```output
@{Id=12692; ProcessName=chrome; CPU=39431.296875}
@{Id=21292; ProcessName=OUTLOOK; CPU=23991.875}
@{Id=10548; ProcessName=CefSharp.BrowserSubprocess; CPU=20546.203125}
@{Id=312848; ProcessName=Taskmgr; CPU=13173.1875}
@{Id=10848; ProcessName=SnapClient; CPU=7014.265625}
@{Id=9760; ProcessName=Receiver; CPU=6792.359375}
@{Id=12040; ProcessName=Teams; CPU=5605.578125}
@{Id=498388; ProcessName=chrome; CPU=3062.453125}
@{Id=6900; ProcessName=chrome; CPU=2546.9375}
@{Id=9044; ProcessName=explorer; CPU=2358.765625}
```

In this example, you can use `Write-Information` to write the top 10 highest
CPU utilization processes from the `Get-Process` object output that has passes
through multiple pipelines.

## PARAMETERS

### -MessageData

Specifies an informational message that you want to display to users as they run a script or command.
For best results, enclose the informational message in quotation marks.
An example is "Test complete."

```yaml
Type: Object
Parameter Sets: (All)
Aliases: Msg

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByValue)
Accept wildcard characters: False
```

### -Tags

Specifies a simple string that you can use to sort and filter messages that you have added to the information stream with `Write-Information`.
This parameter works similarly to the *Tags* parameter in `New-ModuleManifest`.

```yaml
Type: String[]
Parameter Sets: (All)
Aliases:

Required: False
Position: 1
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### CommonParameters
This cmdlet supports the common parameters: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction, and -WarningVariable. For more information, see [about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216).

## INPUTS

### System.Object

`Write-Information` accepts piped objects to pass to the information stream.

## OUTPUTS

### System.Management.Automation.InformationRecord

## NOTES

## RELATED LINKS

[about_CommonParameters](../Microsoft.PowerShell.Core/About/about_CommonParameters.md)

[about_Preference_Variables](../Microsoft.PowerShell.Core/About/about_Preference_Variables.md)

[about_Redirection](../Microsoft.PowerShell.Core/About/about_Redirection.md)

[Write-Debug](Write-Debug.md)

[Write-Host](Write-Host.md)

[Write-Information](Write-Information.md)

[Write-Progress](Write-Progress.md)

[Write-Verbose](Write-Verbose.md)

[Write-Warning](Write-Warning.md)

[Write-Output](Write-Output.md)


