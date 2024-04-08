# what is 0499 ?  
Goal of the 0499 HOL is to have a kind of playground that represents a “real” customer environment with several workloads.

# lab overview
systems / workloads / connectivity 
![image](https://github.com/dell-democenter/dell-democenter.github.io/assets/37048842/167698f2-0d6e-4a2f-9606-dae82823f6ce)

# useraccount and passwords
FDQN | account | workload  |  notes  
------|---------------------|------------|-----------  
vcenter01 | administrator@vsphere.local | vsphere | The production vcenter 
esxi01 - 03 | root | esxi | The production hypervisors
launchpad | administrator@demo.local | YOUR jumpbox is AD controller for “demo.local” and DHCP + DNSserver 
ddve-01 | sysadmin | PPDD | primary protection target 
ddve-02 | sysadmin | PPDD | secondary protection target 
sql01 - 02 | administrator@demo.local | SQL 2019 AAG - treated as bare-metal SQL DB 
vault-esxi 	root 	esxi  	for simulating a CR vault area 



 | Prerequisite | [linkname](pfad, asbolut oder relativ)
1 | relativ | [Module 1](./Module_1.md)
2 | absolut | [Absolut Name ](https://www.absolut.com/de-de/)

das ist ein neuer text
mega !!!  

## Header 1

### Header 2

##### Header 3

Tabelle

Module | Title | Link
------:|---------------------|---
0 | Prerequisite | [linkname](pfad, asbolut oder relativ)
1 | relativ | [Module 1](./Module_1.md)
2 | absolut | [Absolut Name ](https://www.absolut.com/de-de/)


```bash
rm -rf /
```
```Powershell
get-help -module dau
```

