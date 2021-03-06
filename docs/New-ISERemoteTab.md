---
external help file: ISERemoteTab-help.xml
schema: 2.0.0
---

# New-ISERemoteTab
## SYNOPSIS
Create remote tabs in the PowerShell ISE.

## SYNTAX

### Credential
```
New-ISERemoteTab [-Computername] <String[]> [-Credential <Object>] [-Authentication <String>]
 [-CertificateThumbprint <String>] [-ConfigurationName <String>] [-Port <Int32>]
 [-SessionOption <PSSessionOption>] [-UseSSL] [-Profile <String>]
```

### Prompt
```
New-ISERemoteTab [-Computername] <String[]> [-PromptForCredential] [-Authentication <String>]
 [-CertificateThumbprint <String>] [-ConfigurationName <String>] [-Port <Int32>]
 [-SessionOption <PSSessionOption>] [-UseSSL] [-Profile <String>]
```

## DESCRIPTION
This command will create one or more remote tabs in the PowerShell ISE. You could use this to programmatically to open multiple remote tabs. The default behavior is to open tabs with your current credentials. But you can specify a single credential for all remote connections, or prompt for a credential for each connection. You might need this if some of the machines require different credentials.

The command also supports additional parameters from Enter-PSSession.

Be aware that if you specify multiple machines and one of these parameters, such as UseSSL, that parameter will apply to all remote connections.

## EXAMPLES

### -------------------------- EXAMPLE 1 --------------------------
```
PS C:\> New-ISERemoteTab chi-dc01
```

Create a new remote tab for computer CHI-DC01 with default settings.

### -------------------------- EXAMPLE 2 --------------------------
```
PS C:\> Get-Content c:\work\chi.txt | New-ISERemoteTab -credential mydomain\administrator
```

Create remote tabs for each computer in the list using alternate credentials.
This is also the type of command that you could put in your ISE profile script to autocreate remote tabs.

### -------------------------- EXAMPLE 3 --------------------------
```
PS C:\> New-ISERemoteTab dmz-srv01,dmz-srv02,dmz-srv03 -prompt
```

Create remote tabs for each computer and prompt for a unique set of credentials for each.

### -------------------------- EXAMPLE 4 --------------------------
```
PS C:\> New-ISERemoteTab dmz-srv01 -Credential domain\administrator -Authentication CredSSP
```

Create a remote tab for dmz-srv01 with alternate credentials using CredSSP for authentication.

### -------------------------- EXAMPLE 5 --------------------------
```
PS C:\> New-ISERemoteTab dmz-srv01 -ConfigurationName Microsoft.Powershell32
```

Create a remote tab for dmz-srv01 using the 32-bit configuration settings. The display name for this tab would be "dmz-srv01 Microsoft.Powershell32".

### -------------------------- EXAMPLE 6 --------------------------
```
PS C:\> New-ISERemoteTab chi-core01,chi-core02 -profile c:\scripts\remote.ps1
```

Create remote tabs for computers CHI-CORE01 and CHI-CORE02.
Upon connection remotely run the commands in the local file c:\scripts\remote.ps1.

### -------------------------- EXAMPLE 7 --------------------------
```
PS C:\> import-csv s:\computers.csv | where { test-wsman $_.computername -ErrorAction SilentlyContinue} | Out-GridView -Title "Select computers" -OutputMode Multiple | New-ISERemoteTab -Profile S:\RemoteProfile.ps1
```

Import a list of computers and filter those that respond to Test-WSMan.
This list is then piped to Out-Gridview so that you can select one or more computers to connect to using a remote profile script and current credentials.

## PARAMETERS

### -Computername
The name of the server to connect.
This parameter has an alias of CN.

```yaml
Type: String[]
Parameter Sets: (All)
Aliases: cn

Required: true
Position: 1
Default value: none
Accept pipeline input: true (ByPropertyName, ByValue)
Accept wildcard characters: False
```

### -Credential
A PSCredential or user name to be used for all specified computers.
Note that if you specify a credential, it will temporarily be exported to disk so that each new PowerShell tab can re-use it.
The file is deleted at the end of the command.


```yaml
Type: Object
Parameter Sets: Credential
Aliases: RunAs, cred, c

Required: false
Position: named
Default value: [System.Management.Automation.PSCredential]::Empty
Accept pipeline input: false
Accept wildcard characters: false
```

### -PromptForCredential
Use this parameter if you want to prompt for a credential for each connection.
No credential information is written to disk.

```yaml
Type: SwitchParameter
Parameter Sets: Prompt
Aliases: 

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### -Authentication
Specifies the mechanism that is used to authenticate the user's credentials.
Valid values are "Default", "Basic", "Credssp", "Digest", "Kerberos", "Negotiate", and "NegotiateWithImplicitCredential".

```yaml
Type: String
Parameter Sets: (All)
Aliases: auth, am

Required: false
Position: named
Default value: none
Accept pipeline input: false
Accept wildcard characters: false
```

### -CertificateThumbprint
Specifies the digital public key certificate \(X509\) of a user account that has permission to perform this action.
Enter the certificate thumbprint of the certificate.

```yaml
Type: String
Parameter Sets: (All)
Aliases: thumb

Required: false
Position: named
Default value: none
Accept pipeline input: false
Accept wildcard characters: false
```

### -ConfigurationName
Specifies the session configuration that is used for the interactive session.

```yaml
Type: String
Parameter Sets: (All)
Aliases: 

Required: false
Position: named
Default value: none
Accept pipeline input: false
Accept wildcard characters: false
```

### -Port
Specifies the network port on the remote computer used for this command.
To connect to a remote computer, the remote computer must be listening on the port that the connection uses.
The default ports are 5985 \(the WinRM port for HTTP\) and 5986 \(the WinRM port for HTTPS\).

```yaml
Type: Int32
Parameter Sets: (All)
Aliases: 

Required: false
Position: named
Default value: 0
Accept pipeline input: false
Accept wildcard characters: false
```

### -SessionOption
Sets advanced options for the session.
Enter a SessionOption object, such as one that you create by using the New-PSSessionOption cmdlet, or a hash table in which the keys are session option names and the values are session option values.

```yaml
Type: PSSessionOption
Parameter Sets: (All)
Aliases: 

Required: false
Position: named
Default value: none
Accept pipeline input: false
Accept wildcard characters: false
```

### -UseSSL
Uses the Secure Sockets Layer \(SSL\) protocol to establish a connection to the remote computer.
By default, SSL is not used.

```yaml
Type: SwitchParameter
Parameter Sets: (All)
Aliases: 

Required: false
Position: named
Default value: false
Accept pipeline input: false
Accept wildcard characters: false
```

### -Profile
The path to a local file with PowerShell commands to be executed remotely upon connection. Each command in the script must be on a single line. This is a way to run a profile script in the remote session. Here is an profile script example:
    # Sample remote profile script
    cd c:\\
    cls
    Get-WMIObject -class Win32_OperatingSystem | Select @{Name="OS";Expression = {$_.Caption}},@{Name="PSVersion";Expression = {$PSVersionTable.PSVersion}}

Do not use any block comments in your remote profile script. See examples for additional help.

```yaml
Type: String
Parameter Sets: (All)
Aliases: 

Required: false
Position: named
Default value: none
Accept pipeline input: false
Accept wildcard characters: false
```

## INPUTS

### [string]

## OUTPUTS

### none

## NOTES
Last Updated: July 10, 2016 
Author      : Jeff Hicks \(http://twitter.com/JeffHicks\)  
Version     : 1.5.0  

Learn more about PowerShell:
http://jdhitsolutions.com/blog/essential-powershell-resources/


****************************************************************
DO NOT USE IN A PRODUCTION ENVIRONMENT UNTIL YOU HAVE TESTED 
THOROUGHLY IN A LAB ENVIRONMENT. USE AT YOUR OWN RISK. 
IF YOU DO NOT UNDERSTAND WHAT THIS SCRIPT DOES OR HOW IT WORKS, 
DO NOT USE IT OUTSIDE OF A SECURE, TEST SETTING. 
****************************************************************

## RELATED LINKS

[Enter-PSSession]()
[Test-WSMan]()
[New-ISERemoteForm]()


