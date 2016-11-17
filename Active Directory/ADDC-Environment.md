Install-WindowsFeature -Name AD-Domain-Services -IncludeAllSubFeature -IncludeManagementTools

Install-ADDSForest -CreateDnsDelegation:$false -DatabasePath "C:\Windows\NTDS" -DomainMode Win2012R2 -DomainName "mydomain.local" -DomainNetbiosName "MYDOMAIN" -ForestMode Win2012R2 -InstallDns:$true -LogPath "C:\Windows\NTDS" -NoRebootOnCompletion:$false -SysvolPath "C:\Windows\SYSVOL"

New-NetIPAddress –InterfaceAlias “Ethernet” -IPAddress "192.168.0.1" -AddressFamily Ipv4 –PrefixLength 24 -DefaultGateway 192.168.0.254
Set-DNSClientServerAddress –interfaceIndex 12 –ServerAddresses (“192.168.0.1”,”192.168.0.2”)

Add-DnsServerResourceRecordA -Name "intranet" -ZoneName "mydomain.local" -AllowUpdateAny -IPv4Address "192.168.1.3" -TimeToLive 01:00:00