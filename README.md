# workstrongtie


STRONGTIE

timesheet
https://authorizeme.roberthalf.com/s/candidatesignin?language=en_US&eid=jdoluwasanmi%40gmail.com&c=US&d=en_US&e=jdoluwasanmi%40gmail.com&utm_campaign=Candidate-TCAST_Reminders-IC-Email1-NA&g=TqtcfJtjxpxr5cpklBb7YDie64Jb9jlNttYE4ctg&utm_medium=Email&i=a563w000001fdOFAAY&sfi=0033w00003tC7e1AAC&utm_source=LUX-TCAST-Account-Creation&utm_content=create_account_btn
jdoluwasanmi@gmail.com


robert half
jdoluwasanmi@gmail.com


STRONGTIE

inside vm SCANS just like qualys scan

sharepoint -  basic call out schedule



when there is a filled up disc file….you should ask iT Helpdesk for more space




PATCHING


rmt-cli system list —all|grep SCC|wc -l


cat /etc/yum.repos.d/redhat.repo

subscription manager





HOW TO ADD USERS ON DMZ, VMWare
su - svc-rhsatellite
cd Redhat - cd user add - open cat useradd_local.yaml
---
- name: playbook to add users
  hosts: SRV
  gather facts: no
  vars_files:
    - vars/users

  tasks:
    - name: add group
      group:
        name: "{{ item.group }}"
        gid: "{{ item.gid }}"
        state: present
      loop: "{{ users|flatten(1) }}"

    - name: useradd
      user:
        name: "{{ item.username }}"
        group: "{{ item.group }}"
        groups: "{{ item.groups }}"
        comment: "{{ item.gecos }}"
        create_home: true
        home: "{{ item.homedir }}"
        password: "{{ item.password|password_hash('sha512') }}"
        shell: "{{ item.shell }}"
        state: present
      loop: "{{ users | flatten(1) }}"

    - name: force the user to change password
      shell: chage -d 0 {{ item.username }}
      loop: "{{ users | flatten(1) }}"


cd vars (this means variables)
vim users  and add as many users as you like

cat inventory_files (check servers to see how many servers are listed)
 

HOW TO SEE THE NON PROD INVENTORY and PROD INV

cd Redhat - cd patching -  cd inventory_nonprod - vi non-prod

how to run playbook for patching
ansible-playbook -i inventory_nonprod







SUSE PATCHING
curl http://rmtpmprd.simpsonmfg.com/tools/rmt-client-setup --output rmt-client-setup.sh; bash rmt-client-setup.sh https://rmtpmprd/simpsonmfg.com










FTP NFS NTP SAMBA
ftp server is in the DMZ

PO shares is in drive L (no access yet)
login info

when someone need samba access…it has to go thru S4hA1 server

NFS/CIFS/ftp/Samba app is on S4HA1prd








BACKUP GOES TO SCSI 3

ls -ld /usr/sap


then share the info to mario to configure the SCSI Controller (I will tell him what kind of DISC OS we are targeting)so he will change the hard rive SCSI, then he will reboot…then we login into the vm toseee that there is no issue, after that, we will bring up the db system



REDHAT CONTENT VIEW / ENABLED REPO
BC29 and BCVN MUST NOT BE PATCHED

CREATE CONTENT VIEWS PRODUCT FOR: 
RHEL 8.4
RHEL 7.9
CENTOS 8

Go to CONTENT VIEWS
Copy the last/latest product for each version
Select Action —— Click Copy Content View
Then also highlight the name of the content view on the top left as the NEW NAME and click on CREATE
then click on Publish New Version , then SAVE (you can put description if you want.)
then it will start Publishing a new version
After it finish publishing…
click on CONTENT VIEWS and go to the NEW PUBLISHED VERSION to promote it… click on  PROMOTE (click on  development)
then go to HOST and click on CONTENT HOSTS to assign the new published and promoted version to the hosts
Click on each server to assign the hosts.. go to Content View inside the server to assign new host.
and save it.

Then do the same on the remain rhel and centos version


GO TO CONTENT VIEW / CLICK ON WHAT VIEW YOU NEED ——  COPY THE VIEW  AND GO BACK TO CONTENT HOST TO ASSIGN IT—CLICK ON the host in content host —— go to content view in side of the host and assign it …the host will have access to it and you can confirm it in the terminal (sudo su -  then type subscription-manager repos)

another method:  for content… inside redhat satellite customer portal…go to content ——create a content view——copy the latest one for rhel8(click on it)——— at the top right… select action-copy content view…click new name and type in a new nam(SSD_RHEL_8Server_101021)—create—publish——save…then go to content host and allocate the published content view to each host.

another easier way is to click on the latest version and click on publish, change the date and it will create another version…eg version 2


REDHAT SATELLITE UPGRADE
REDHAT Satellite upgrade environment checks …we will upgrade in the upcoming redhat patch

connected, no, no, yes, yes …move to next and follow the command line/page (1595963)


How to apply the bundles to the servers
1. Assign it to the host
2. run the playbook USING ANSIBLE

PATCHING USING ANSIBLE

go to server terminal (RHLAPPRD…then sudo su - to root, then su - svc-rhsatellite)… and cd Redhat … cd patching (ll to see the list) … 
go to cd inventory to see all the servers
then run shutdown.yml
go to VMWARE and create snapshots of all the servers AND NOW BRING THE SERVER BACK ON
THEN run yum.update.yml   (ansible-playbook --ask-become-pass yum-update.yml)
run another playbook called (reboot-os.yml)

and report


(to seee what was last installed using sensible… ansible all -i inventory_nonprod/non-production -m shell -a’ rpm -qa --last|head -n 5’)


PROCESS TO REGISTER SERVER IN SATELLITE   (dev qas happens within daytime)
build server
install OS then go to content on satellite… select activation key - choose ssd rhel standard(5days license)
ssd_RHEL&_Premium (24/7support)
SSD_RHEL_Development is free

click on it… copy the activation key…

go to server terminal (RHLAPPRD…then sudo su - to root, then su - svc-rhsatellite)… and type this command


how to register a new server on satellite
go to activation key
pick standard/development/premium..
copy the registration command above
go to the terminal…enter the command

subscription-manager unregister 
subscription-manager clean
 paste what you copy from redhat satellite activation key account)




SCC (SCSI Controller) setup on SAP DB
Hostname: S4HD1SBX 
sudo su - h4xadm
type command: HDB info
then stop the HDB by using command: HDB stop   

if it is app… stopsap 

then, I will get the list of the block by typing : lsblk

then sudo shutdown -P now

now look thru the lsblk for hana db/db backup
 
sde was set to be iscsi 1

 s4hdbprd




SUSE PATCHING
RUN THIS EVERY TWO MONTHS (./rmtrepo mirror; ./rmtrepo sync)

stop the app/db (stopsap or sapcontrol -nr 03 -funtion Stop) for DB (HDB stop)
power off the server
snapshot
poweron
patch
reboot
when bringing up the server
bring up the DATABASE first (HDB start)before the APP (startsap or sapcontrol -nr 03 -funtion Start)




log in to rmtpmprd
cd /var/lib/rmt
cd public/repo/SUSE
do backup (cp -a SUSE SUSE 11112021)

then go to 
cd /opt/scripts
cd patching 
cat rmtrepo
then run ./rmtrepo  (do this like a day or two prior)

then reach out to it helpdesk to request approval for patching

subject: Patching DEV, SBK, PRJ
Assign: joluwasanmi

This ticket is to track patching on non-production servers: 

s4hapdev
s4hapdee

Regards

John

you will get a respond with an incident number… when you are done.. you put “”patching done”” in the note and closer information will be patching done also.


then start the patching by
cd SUSE/startup_scripts/
vim startup-main.yml to edit the host to what you have in prod (s4)——also comment out the remaining host names 
then run the playbook..
ansible-playbook -i inventory_prod/production startup-main.yml 







REDIS PATCHING IS DIFF
THEY ARE MANUAL
steps are in the sharepoint


QUESTIONS FOR PRASAD:
CAN YOU GIVE ME THE ACCESS TO REDHAT SATELLITE?
HAVE YOU BEEN ABLE TO MODIFY THE ANSIBLE SCRIPT FOR SUSE? 

do we have a video kit for ad authentication and sssd usage?




SUSE ACCOUNT
joluwasanmi
Jesu86Seun!.

rhlapprd: joluwasanmi/Jesus123!.




cat list$|awk -F’ ‘ (‘ print $1’)    will copy paste and list




SOX AUDIT: LINUX ACCESS REVIEW
log in to rmtpmprd --- cd /opt/scripts — add your key with this command (ssh-agent bash) ——then add your key (ssh-add)—cd /opt/scripts——history —— cd sox_control_scripts.v2

(always remember to update the sox list…add or delete servers)

copy script tp sox servers (for SRV in ‘cat sox_list’ ; do scp sox_script_06302020 $SRV: ; done)
execute the script on each SOX server: for SRV in ‘cat sox_list’ ; do ssh -q -t $SRV ‘sudo ./sox_script_06302020’ ; done
once you finish running the script… you will have a new file

GATHER EVIDENCE: for SRV in ‘cat sox list’ ; do scp $SRV:$SRV-07212021.zip . ; done

so create a folder (mkdir 10062021)
move all the zip file into the folder: (mv *.zip 10062021)

so copy the zip file …place it in a xls sheet and attach it to the incident.




SAP AUDIT: SQL SERVER RESTORE (20:25 in the video)

Log on to rubrik …242rubrik—log in —search for the server above(dvronrsprd01)—go pick a date (make sure the date is uniform across the rest of the servers(two weeks prior))

If Rama says I need a file 20 20 logs..you will go to s4padm under s4ha1prd server —go to cd /usr/sap/S4P/audit——ll to find it


SFTP
s4HA1PRD This is used for inbound






ansible-playbook os-gru	b.yml -i inventory-hana --syntax-check

ansible-playbook os-gru	b.yml -i inventory-hana —tags validate

ansible-playbook os-gru	b.yml -i inventory-hana —tags grub
ansible cicd  -m shell -a 'rpm -qa --last|head -n 5'

bash shell
[root@dns ~]# for i in $(<cicd); do echo $i; ssh -k -tt -o StrictHostKeyChecking=no $i 'sudo bash -s' < update.sh;done

ansible-doc -l | grep -i udevadm
ansible-doc copy or blockinfile



How to know change incident / 


ansible all —list-host
ansible s4hdbdev -a ‘lsblk’





***OS RECOMMENDATION***
cd ansible/suse
vim ansible.cfg  (inventory=inventory.hana or whatever inventory you want to run)
go to the ansible script to edit the hostname: vim os-grub…
3. hosts: (the name of the server)
then use (ansible s4hdbdev -a ‘lsblk’) to see the blocks you have on the server
then comment out if it is more or uncomment if it is less

so run 
ansible-playbook  





EXTEND ALREADY FULL LVM PARTITION
Take note of the lvm name ——df -h  (you can check if you can also clean the files tho)
check if the file system is an LVM partition — lvs(take note of the name /usr/sap/s10D= /dev/mapper/lv-vg)
check if there is VG space on the file system—— vgs 
add from the space(20G) to /usr/app/s10D (lvextend -v -L +10G /dev/app/vg)
resize the filesystem 
for XFS (xfs_grow /usr/app/s10D)    for EXT4 (resize2fs /dev/vgdata/lvdata
verify: df -h






HOW TO CREATE A NEW LVM PARTITION
find out which disk is new (lsblk)
fdisk /dev/sdf
n
p first sector and last sector
t
8e
w

pvcreate /dev/sdf
vgcreate vgdata /dev/sdf (verify it with vgs)
lvcreate -l 10G -n lvdata vgdata (verify with lvs)
format the partition as xfs (mkfs.xfs /dev/vgdata/lvdata)
mount your partition to a folder (mount /dev/vgdata/lvdata -o nobarrier,noatime,logsize=256k /dev/vgdata/lvdata /app)
verify by (df -h | grep /app and mount | grep /app)
 add an entry to /etc/fstab to be persistence across reboots (/dev/vgdata/lvdata 	/app		xfs		rw,nobarrier,noatime,logbsize=256k 	1 2)


HOW TO RESIZE tmpfs
1. first check the current size for /dev/shm on tmpfs         df -h
2. to change, we need to remount and specify the size (mount -o remount,size=38G /dev/shm)
3. check if the size change (df -h)
4. to make it persistent …add it or edit in /etc/fstab (none  /dev/shm    tmpfs     defaults, size=38G   0 0)
5. save and exit




HOW TO MOUNT NFS
1. CHECK TO see if NFS is running (systemctl start nfs.service and systemctl start rpcbind.service ..also enable and check status)
2.  create a directory to where you want to mount the share (mkdir /mediaapp)
3. mount the share to directory NFS: 290library.johnlord.com:/library  /mediaap/    (mount -t nfs 240library.johnlord.comm:/library  /mediaapp/)
4. make it persistent across reboot by adding entry to /etc/fstab (240library.johnlord.comm:/library           /mediaapp         nfs       defaults,rw    0  0)
5. save and exit



HOW TO CONFIGURE NTP ON SUSE YAST2
This document only covers configuration of NTP client on a linux server…assuming you already have an NTP server to point to

johnlord NTP server    10.192.168.80

login to your suse server and change to root user
1. yast2
2. select Now and on Boot, then ADD
3. Select Server then Next
4. add the IP address of our NTP server, then Test. Once server is reachable, click OK
5. server should show up on your list of ntp servers. do the same procedure to the rest of the NTP server…they are applied instantly and no reboot is needed



