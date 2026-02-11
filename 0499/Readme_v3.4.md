># **click here for [actual issues](#actual-issues) | click here for [release notes](#release-notes-0499v34) 0499v3.4**

# what is HOL-0499 ?  
### objective is to have a playground that represents a more or less “realistic” customer environment with several workloads.
### on this playground we give you the chance to install and configure Dell Technologies products from scratch on your own to gain experience !

# lab overview
### systems / workloads / connectivity 
<img width="1569" height="865" alt="image" src="https://github.com/user-attachments/assets/04a50b19-9a9c-4183-923d-a53bd59860aa" />






### Note on VLAN2
VLAN2 is for storage+backup traffic "prepared". VLAN2 has the ip subnet 192.168.2.x/24  
esxi servers already have kernel interfaces. ppdm is already using it for DR backup. powerstore has already connect with data interfaces.  
connect your systems, like your ddve, to VLAN2 if traffic "separation" is wanted ;-)

# FQDN and DNS suffix  
***.demo.local** is the usual DNS suffix   
naming policy is: use ALWAYS FQDN !!!

# account and passwords  
Username depends, as always, on the workload or the device you want to login or manage   
Rule of thumb for password is always: **Password123!**

Exceptions listed here: 
FDQN | account/password | workload  |  notes  
------|---------------------|------------|-----------  
vault-ppdm | root/changeme | ppdm in the vault | OS level
vault-ppdm | admin/@ppAdm1n | ppdm in the vault | app level
nve-1 | root passphrase "Password123" | NVE upgrade | without exclamation mark

# workloads
FDQN | account | workload  |  notes  | runs on
------|---------------------|------------|-----------|-------------  
vcenter01 | admin@vsphere.local | vsphere | The production vcenter | democenter level
esxi01+02 | root | esxi | broadcomm hypervisors | democenter level
ntnx-node | root | Nutanix WebUI  | nutanix hypervisor aka. AHV | democenter level
ntnx-node-cvm | nutanix | nutanix CVM | controls the AHV node | nutanix
ntnx-prismcentral | admin | nutanix prismcentral | like a vcenter | nutanix
nutanix-move | nutanix | nutanix move | migrate vm´s from other hypervisors to nutanix | nutanix
proxmox1 | root | proxmox web UI | is doing KVM and LXC | democenter level
pbs | root | proxmox backup server | is doing backup/restore | democenter level
nas | admin | NFS Datastore for vsphere | rocky linux and zfs | democenter level
launchpad | administrator@demo.local | YOUR jumpbox is AD controller for “demo.local” and DHCP + DNS | | democenter level
ansible | admin | ansible jumphost for ansbile CLI automation | filled with wonderful automation magic from karsten | vmware
ddve-01 | sysadmin | PP DataDomain | primary protection target | democenter level
ddve-02 | sysadmin | PP DataDomain | secondary protection target | democenter level
sql01+02 | administrator@demo.local | SQL 2019 AAG  | |	running on hyper-v 
ddmc |	sysadmin | ddmc | NO smart scale included anymore ! | proxmox 
dpa |	administrator  | DPA discontinued | | proxmox 	
dpc |	administrator@dpc.local  | DPC discontinued	| | nutanix
exchange1+2  |		administrator@demo.local  |		Exchange 2019 DAG  |	 Clustered mail system | vmware
file  |		administrator@demo.local  |		Win 2022 file server  |		Central file server | vmware
hana01  |		H01adm  |		hana hxe 2.0 SPS02  |	 	SAP HANA  | vmware 
ocpnode0+1+2  |		kubeadmin  |		openshift cluster  | Kubernetes + virtualization from RedHat |  democenter level
powerscale  |		root  |		Isilon oneFS | treated as NAS workload / joined demo.local AD  | vmware
powerstore-1+2  |		admin  |	PowerOS | treated as NAS workload / joined demo.local AD  | vmware
scvmm  |		administrator@demo.local  |		system center virtual machine manager  |	old stlye UI fpr central managing vm´s | hyper-v 
wac  |		administrator@demo.local  |		Windows Admin Center  |		nice UI for central managing windows machines  | hyper-v
nve-1  |		administrator  |		Networker  |	legay dataprotection 	| vmware
nve-1-vproxy  |		admin  |		vproxy 	| | vmware
ora1  |		root or oracle | Oracle 19c DataGuard active |	use DataStudio Client to access DB with UI | vmware
ora2  |		root or oracle | Oracle 19c DataGuard standby |	use DataStudio Client to access DB with UI | vmware
db2 | root or db2inst1 | IBM DB2 |use DataStudio Client to access DB with UI | vmware
my-sql01 | root | mysql | use DbGate to access DB with UI | vmware
mariadb-1 | root | mariaDB | use DbGate to access DB with UI| vmware
postgres-1 | root | PostgreSQL | use DbGate to access DB with UI| vmware
mongodc-1 | root | mongoDB |  use DbGate to access DB with UI| vmware
ppdm-1  | admin  | PP Data Manager  | next gen dataprotection with genAI | vmware
ppdm-1-search | NA | PPDM search node | | vmware
ppdm-1-report | NA | PPDM report node | | vmware
sql03+04 | administrator@demo.local | SQL 2022 AAG | |  vmware   
msr | admin | multi sytems reporting | ppdm-1 already onboarded | proxmox  
***| *** | *** | *** | ***
vault-esxi | root | esxi | for simulating a CR vault area  | demonceter level
vault-vcenter |	admin@vsphere.local | vSphere for the vault|The separated vcenter in the vault | vmware  
vault-ppdm | root/changeme | ppdm for the vault | ppdm is waiting in “install screen” | vmware
vault-ddve | sysadmin | PPDD for the vault | the separated protection target  | vmware
cr | crso + cradmin | PPCR for the vault | vault CyberRecovery Manager | vmware
cs | admin |	CyberSense  | Optional deep forensic  | vmware 


# systems in detail  
## launchpad  
has the function to be the initial place for accessing the 0499 lab environment. it runs the AD controller role for "demo.local". 
it runs an **DHCP** and **DNS** server. DHCP Server lease pool is from 192.168.1.100 up to 192.168.1.199. all adresses below 192.168.1.100 are manual and YOU have to doublecheck for duplicate IP´s and correct DNS entrys. DHCP server updates all hostnames from any linux/windows to the DNS server if the client is able to transfer his hostname to DHCP. means if you deploy a linux or windows vm that hostname will listed and updated in the DNS server automatically, result is a working name reolution in your lab.
any manual configured IP have to to manual maintained in the DNS server.  

### XCA tool
is for generating your own SSL certificates. rootCA for democenter is alreday pushed via GPO. so every windows machine that is member of demo.local AD trust your self created SSL certificates. on linux boxes you have to this manually depended on the used distribution.   
how to use XCA: https://hohnstaedt.de/xca/  

### mremoteNG
contains pre-configured SSH and RDP sessions for almost all workloads and systems

### browser
MS EDGE is default browser. Firefox ist installed but not everywhere supported, so just for doublechecking for UI "issues". 

### DbGate
really nice tool for accessing/editing/modifying databases like mariaDB / mySQL / PostgreSQL / MongoDB with a easy to use UI interface.
all DBs entry already provided

### Data Studio Client (IBM)
nice java UI for accessing the DB2 single node instance and the oracle dataguard cluster.
for those how wants to see that the a DB is alive/mounted and able to create entrys.

### SAP HANA Studio
nice java UI for accessing the HANA HXE single node instance.

### 2FAGuard
nice TOTP tool for windows to scan your QR codes when activating 2FA for user of your choice
dont use your own TOTP tzool on your mobile ;-)

###WinSCP 
well known tool for transferring files to linux systems.

## vcenter01 (by broadcom)
use either "admin@vsphere.local" or "windows session authentication". user "administrator@vpshere.local" is locked due to license agreement with broadcom. if you need to create "user accounts" in vsphere use AD user from "demo.local". 

## ansible
a linux host preconfigured to run automation magic with ansible playbooks.

## mail /exchange1 / exchange2
this exchange dag let you receive and send email just internally. it not (yet) relaying any SMTP mails.
the name of IP less DAG is "mail.demo.local" residing on two nodes "exchange1" and "exchange2"
some system are already configured to send email like ASUPś and so on. try to configure it for your workload.

## ppdm-1
is fresh installed and has just the "vcenter01" asset configured.
it is doing TSDM protection of all vsphere vm's, so if you mess something up, stay cool, you have a backup.
report and index engine are already deployed.

## nve-1
is fresh installed and has just the "vcenter01" for image backup configured.
it is doing VADP protection of all vsphere vm's, so if you mess something up, stay cool, you have a backup.

## ddmc
fresh deployed, nothing configured

## DPA
fresh deployed, nothing configured

## DPC
fresh deployed, nothing configured

## powerstore´s
fresh deployed, initial network cinfiguration done. both systems are connected for replication and metro. 
intend to be used when demonstrating synergy between dell primary storage <> backup storage, orchestrated by PowerProtect

## ontap simulator
ontap 2 node cluster with no shared backend (simulator limit)  
managing with virtual cluster name ontap-cluster.demo.local or IP 192.168.1.70  
some storage virtual machine for CIFS / NFS /iSCSI are already prepared  
try some DNAS backup with ppdm or import external ontap storage with powerstore

## powerscale
fresh deployed, joinded the ADS demo.local domain. has some file in the \\powerscale\data SMB share. intend to be used for demontrating the DNAS workload in PPDM.

## openshift
3 node openshift cluster with virtualization. intend to be used for demontrating the K8s and vm workload in PPDM

## nas 
is running rocky linux and doing the NFS datstore job for the esxi servers. DONT TOUCH ;-)  

## scvmm (system center virtual machine manager)
the old style way to manage vm´s
can be pimped with our networker plugin ;-)

## wac (windows admin center)
the "option" to have a nice HTML5 UI for managing windows systems.
**MUST** be accessed by edge, it's not working with firefox

## nutanix 
is a single node nutanix "cluster". running nutanix´s hypervisor aka. AHV. (some kind of KVM).   
every nutanix node runs a control vm (CVM) that controls the underlying node. limited reources avalaible...

## prismcentral 
some kind of "manager of managers" aka. vcenter. prismcentral aka. PC can control multiple nutanix clusters.  

## proxmox 
is a single node proxmox cluster. can run KVM vm´s and linux containers. for demoing what proxmox is and most important whts it´s NOT.   
pbs ande backup schedule is already configured and onboarded.

## pbs
proxmox backup server aka. PBS. a product that is specific designed to protect proxmox environments. it´s doing ceompression and dedupe, always.


# actual issues 
## openshift issues here (SOLUTION)  
OADP Operator not deployed. 

### Option 1, use new OADP version 1.5.3

On PPDM host, open the file /usr/local/brs/lib/cndm/config/k8s-dependency-versions-app.properties.
```bash
# Path and desired values
FILE="/usr/local/brs/lib/cndm/config/k8s-dependency-versions-app.properties"
VER="1.5.3"
CHAN="stable"

# Ensure the file exists
[ -f "$FILE" ] || touch "$FILE"

CHANGED=0

# --- k8s.oadp.version ---
KEY="k8s.oadp.version"
VAL="$VER"
CUR=$(awk -v key="$KEY" '
  $0 ~ "^[ \t]*#?[ \t]*"key"[ \t]*=" {
    line=$0
    sub(/^[ \t]*#?[ \t]*/,"",line)
    sub(/[ \t]*=[ \t]*/,"=",line)
    split(line, a, "=")
    print a[2]
    exit
  }' "$FILE")
if [ "$CUR" != "$VAL" ]; then
  tmp="$(mktemp)"
  awk -v key="$KEY" -v val="$VAL" '
    BEGIN{f=0}
    {
      if ($0 ~ "^[ \t]*#?[ \t]*"key"[ \t]*=") {
        if (!f) { print key"="val; f=1 }
        next
      }
      print
    }
    END { if (!f) print key"="val }
  ' "$FILE" > "$tmp" && mv "$tmp" "$FILE"
  CHANGED=1
fi

# --- k8s.oadp.channel ---
KEY="k8s.oadp.channel"
VAL="$CHAN"
CUR=$(awk -v key="$KEY" '
  $0 ~ "^[ \t]*#?[ \t]*"key"[ \t]*=" {
    line=$0
    sub(/^[ \t]*#?[ \t]*/,"",line)
    sub(/[ \t]*=[ \t]*/,"=",line)
    split(line, a, "=")
    print a[2]
    exit
  }' "$FILE")
if [ "$CUR" != "$VAL" ]; then
  tmp="$(mktemp)"
  awk -v key="$KEY" -v val="$VAL" '
    BEGIN{f=0}
    {
      if ($0 ~ "^[ \t]*#?[ \t]*"key"[ \t]*=") {
        if (!f) { print key"="val; f=1 }
        next
      }
      print
    }
    END { if (!f) print key"="val }
  ' "$FILE" > "$tmp" && mv "$tmp" "$FILE"
  CHANGED=1
fi

# Optional: restart only if something changed
[ "$CHANGED" -eq 1 ] && cndm restart

```


Onboard /  Discover K8S , then patchaaprove the installplan


onboard ppdm:

```bash
export PPDM_RBAC_SOURCE=https://raw.githubusercontent.com/bottkars/ppdm-rbac/19.22.0-24/
ansible-playbook ~/workspace/ansible_ppdm/130.1_playbook_rbac_add_k8s_to_ppdm.yaml
```



pattch installplan
```bash

#!/usr/bin/env bash
set -euo pipefail

NS="velero-ppdm"

echo "Waiting for a pending InstallPlan in namespace: $NS ..."
while true; do
  # Grab the first not-approved InstallPlan
  IP_NAME=$(oc get installplan -n "$NS" -o jsonpath='{range .items[?(@.spec.approved==false)]}{.metadata.name}{"\n"}{end}' | head -n1)
  if [[ -n "${IP_NAME:-}" ]]; then
    echo "Found pending InstallPlan: $IP_NAME"
    break
  fi
  sleep 5
done

echo "Patching InstallPlan to approved=true ..."
oc patch installplan "$IP_NAME" -n "$NS" --type merge -p '{"spec":{"approved":true}}'

echo "Done."

```

### Option 2, using OADP from old Catalog (1.4.3 for openshift < 4.19)
The OADP we use is Part of the 4.17 Catalog which is in a differnt catalogue source. Therefore we need to patch the source, and then manually approve the Installplan ( Or Checlk UI to do it :-) )  
```bash
oc patch subscription redhat-oadp-operator -n velero-ppdm --type=merge -p '{"spec":{"source": "redhat-operators-417"}}'

oc patch installplan $(oc get installplan -n velero-ppdm -o jsonpath='{.items[0].metadata.name}') -n velero-ppdm --type merge -p '{"spec":{"approved":true}}'
```

## DPC login in UI not possible (SOLUTION)
- ssh into DPC via mremoteNG
```bash
sudo /usr/local/dpc/bin/dpc start
```
- check that all services are in state "active" (except FIPS ;-)
```bash
sudo /usr/local/dpc/bin/dpc status
```
- wait 1-2 minutes and try to login in UI again

## CR login in UI not possible (SOLUTION)
- ssh into CR via mremoteNG
```bash
sudo /opt/dellemc/cr/bin/crsetup.sh --verifypassword
```
- use always the default lab Password123!
- regenerate the expired certificates
```bash
sudo /opt/dellemc/cr/bin/crsetup.sh --securereset
```
- wait 1-2 minutes and try to login in UI again

## no webUI on nutanix or prismcentral - nutanix CVM not starting (SOLUTION)  
- due to not graceful shutdown on nutanix AOS node the related CVM starts not sucesfully, so no ping or SSH into CVM possible
## step #1 
- ssh into ***ntnx-node-CVM***  via mremoteNG as user nutanix
```bash
 disk_operator mark_disks_usable /dev/sd?
```
this forces the CVM node to accept and reuse all existings disks  
## step #2 
- ssh into ***ntnx-node***  via mremoteNG as root
```bash
 echo b >/proc/sysrq-trigger
```
this will do a "hard reset" - wait 90 seconds and try to ping or get access to the nutanix CVM via mremoteNG  
when the CVM is accessible via SSH it will take at least 5 min aka. 300 seconds to start all nutanix services completly  
you can double check in a SSH session as user nutanix on the CVM with:
```bash
 cluster status
```
it will take additional 10 minutes aka. 600 secs to boot up primscentral vm.   
be patient.....

## ontap cluster not starting (SOLUTION)  
- due to not graceful shutdown node shows __"Internal error: Cannot open corrupt replicated database."__  
- ssh into ***BOTH*** ontap nodes via mremoteNG
```bash
set diag
```
answer with "Y"  
```bash
system configuration recovery node mroot-state clear -recovery-state all 
```
answer with "Y"  
wait 60 seconds and try again access to ontap-cluster web UI
if web UI still not accessible after 60 seconds do a "restart guest OS" via vcenter webui of ***BOTH*** nodes  





## vm's did not get DHCP ip adress
- networking issue in the underlying democenter infrastructure.
- cancel the lab and deploy a new one


# release notes 0499v3.4
## new
- 2FAGuard tool for scan TOTP tokens
 	
## changed
- moved some workloads from nutanix to proxmox
- Updated SRP stuff to latest releases

## removed

# release notes 0499v3.3
## new
- Nutanix Prismcentral aka. PC v7.3 runs now per default on nutanix cluster
- MSR ist now pre-installed and runs on nutanix
- proxmox PVE (promox virtual environment) v9.0 installed
- proxmox PBS (promox backup server) v4.0 installed
 	


# release notes 0499v3.3
## new
- syslog server
 	
## changed
- Updated Openshift to 4.20.3
- Updated DDVE/DDMC to 8.6
- Updated PPDM and CRS to 19.22
- Updated CyberSense
- 

## removed

# release notes 0499v3.3
## new
- Nutanix Prismcentral aka. PC v7.3 runs now per default on nutanix cluster
- MSR ist now pre-installed and runs on nutanix
- proxmox PVE (promox virtual environment) v9.0 installed
- proxmox PBS (promox backup server) v4.0 installed
 	
## changed
- Nutanix AHV updated to AOS 7.0 
- Hycu R-Cloud updated to v5.2
- nutanix move updated to 5.6.0
- hycu updated to v5.2
- moved DPA from vmware to nutanix
- moved DDMC from vmware to nutanix

## removed
-  esxi node3  



# release notes 0499v3.2
## new
- Nutanix AHV one node cluster with 
- Hycu R-Cloud
- mariadDB
- PostgreSQL
- mongoDB
- DbGate offer nice UI for DB´s
- rocky 9 / centos 9 / SLES15 / win2025 templates  
- ontap 9.15 simulator 2 node cluster  
- openshift virtualization  
- openshift ansible automation platform  
 	
## changed
- moved all hyper-vm´s from SMB to CSV datastore
- moved all vault-esxi vm´s from local datastore to nfs-datastore
- updated NW / DPC / DDMC / DPA / PPDM / CR / CS to latest version (march 2025) 
- updated Windows Admin Center  
- updated launchpad to windows 2025  
- updated hyper-v nodes to windows 2025  

## removed
-  Rancher  
-  vault-esxi local datastore

