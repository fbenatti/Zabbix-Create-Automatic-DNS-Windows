-Configure the Zabbix-Agent in Windows Server:

--------------------
Server=Server of Zabbix

Hostname={{Hostname Server}}

StartAgents=5

DebugLevel=3

LogFile=C:\Zabbix\zabbix_agentd.log

Timeout=30

- Configuration for DNS in Windows 2012.

UserParameter=cria-dns[*],powershell Add-DnsServerResourceRecord -A -IPv4Address $1 -Name $2 -ZoneName $3 -AgeRecord -CreatePtr

UserParameter=remove_direto-dns[*],powershell Remove-DnsServerResourceRecord -Name $1 -RRType A -ZoneName $2 -Force

UserParameter=remove_reverso-dns[*],powershell Remove-DnsServerResourceRecord -Name $1 -RRType Ptr -ZoneName $2 -Force

UserParameter=zone-dns[*],powershell Get-DnsServerZone

- Configuration for DNS in Windows 2008.

UserParameter=cria-dns[*],dnscmd.exe /RecordAdd $3 $2 /CreatePTR A $1

UserParameter=remove_direto-dns[*],dnscmd.exe /RecordDelete $2 $1 A /f

UserParameter=remove_reverso-dns[*],dnscmd.exe /RecordDelete $2 $1 PTR /f

UserParameter=zone-dns[*],dnscmd.exe /enumzones

--------------------

- Configure Value Mapping in Zabbix: 

Name: Automation DNS:

Value map:

1 => DNS registration created succesfull.

2 => Error in creation do dns record.

3 => DNS registered already.

4 => Error in removing the direct appointment.

5 => Error in removing the reverse.

6 => Could not find reverse zone.

7 => DNS Updated Successfully.

8 => Error in SNMP query.

9 => Error running the command.

*Change this variables in script: create_dns

servidor_dns = "10.0.0.1" (Your DNS Server)

zonename = "fbenatti.com" (Your ZoneName)


Export template XML to Zabbix (zbx_auto-dns_templates.xml)

