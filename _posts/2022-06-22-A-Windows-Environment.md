---
layout: post
title: A Windows Environment
subtitle: What the Powershell!?
---

As per the outlined schematic in the last post, I started to build a Windows lab environment. After a little research (namely watching the talented [John Hammond](https://www.youtube.com/watch?v=pKtDQtsubio&ab_channel=JohnHammond) learning how to set up a Windows Environment much like the one I had in mind), I came out with a working understanding of where to start and what to do. 

## WinServer R2 2022

To access server config from the shell, type `SConfig` at anytime from PowerShell. Its worth noting that PowerShell (PS) remoting is enabled by default on WinServer 2022. The server was set a static IP of `192.168.1.100` and the DNS server was set to point to itself.

### Installing Active Directory

To search through windows feature, the `Get-Windowsfeature` command is used. It can be piped to filter outputs, for example:  
```powershell
Get-Windowsfeature | ? {$_.Name - Like "AD*"}
```
To find all mentions of an Active Directory module/feature. AD is installed via the command:
```powershell
Install-WindowsFeature AD-Domain-Services -IncludeManagementTools
Import-Module ADDSDeployment
Install-ADDSForest
```
Active Directory Domain Service (ADDS) is now installed on this server. Follow the prompts and the server will then restart.

Domain `roghidev.org` was created on the DC.

It is important to note that when AD gets installed, the DNS server gets reset back to localhost (`127.0.0.1`). This can be checked via `Get-DNSClientServerAddress` to find the interface the server is using for the DNS server. The DNS server is then set via `Set-DNSClientServerAddress -Interface 5 -ServerAddress 192.168.1.100` (remember that the DNS server is pointing back to the DC so that other workstations on the network can find the Domain Controller/connect to it).

## Windows 11 Workstations

### Lab Management PC

Stock Windows 11 can't remote to a server that hasn't been added to the domain the host belongs to. The management PC hasn't been added to the domain `roghi.dev` yet, so the server needs to be added to the list of trusted hosts in order to establish a remote PS connection.

The WinRM service needs to be started. It can be started by proxy using `Enable-PSRemoting` but other research has shown that `Start-Service WinRM` also works. 

To set an IP as part of the trusted hosts use `set-item wsman:\locahost\Client\TrustedHosts -value {ip}` where the IP of the DC is `192.168.1.1100`. Once this is done, a new PS session can be opened and accesed via `New-PSSession -ComputerName {ip} -Credential (Get-Credential)` where `(Get-Credential)` is a PS CMDlet that allows the parsing of credentials via a GUI application. 

**TODO** *Figure out how to fix a broken PSSession. Changed hostname on the DC and got a broken PSSession. Had to remove the PSSession and create a new PSSession with the same IP, this worked (note, `Enable-PSRemoting`) had to be run again on the DC for this to work...? Why...?*

The MgmtClient IP was set to a static IP of `192.168.1.99`

The PSSession can be assigned to a variable to make calling the same PS Session easier:
```powershell
$dc = New-PSSession 192.167.1.100 -Credential (Get-Credential)
```
Once the credentials have been specified on first login, the variable can be called with `Enter-PSSession` and piped with other commands to have easy access to the DC from the MgmtClient. Its worth noting that any CLI GUI tools used by WinServer 2022 will break in PSSession. This means that  whenever these tools need to be used, the server will need to be managed locally rather than remotely.

An example use of PSSesessions can be found below. The variable `$dc` is a current PSSession to the DC from MgmtPC. 
```powershell
# Copies a config file from the MgmtClient to C:\Windows\Tasks on the DC
Copy-Item .\config -ToSession $dc C:\Windows\Tasks
```

**TODO** *Check is there is a workaround for the above*



### Client 1

Client1 IP was kept as DHCP so as to simulate a live client on the network and ensure the DNS on the DC is working correctly. The client was domain joined via PowerShell using the following command:
```powershell
Add-Computer -DomainName roghidev.org -Credential roghidev\Administrator -Force -Restart
```

**TODO** *Check if the credential needs to be changed or if the default domain administrator remains valid after the initial join.*