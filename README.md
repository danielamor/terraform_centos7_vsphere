# terraform_centos7_vsphere
Terraform Centos 7 template build for vsphere

# Terraform
## Install terraform management vm
```
yum install -y unzip
curl -O https://releases.hashicorp.com/terraform/0.11.9/terraform_0.11.9_linux_amd64.zip
unzip terraform_XXX.zip -d /usr/bin/
```
# VMWare
## Preparation VMware template
Create centos 7 VM with 2 CPU, 1G memory, 10G HDD
Update machine and install required packages
```
yum update
yum install -y wget
```
Install open-vm-tools-deploypkg
```
cd /tmp
wget https://packages.vmware.com/tools/legacykeys/VMWARE-PACKAGING-GPG-DSA-KEY.pub
wget https://packages.vmware.com/tools/legacykeys/VMWARE-PACKAGING-GPG-RSA-KEY.pub
rpm --import VMWARE-PACKAGING-GPG-DSA-KEY.pub
rpm --import VMWARE-PACKAGING-GPG-RSA-KEY.pub

vi /etc/yum.repos.d/vmware-tools.repo
    [vmware-tools]
    name = VMware Tools
    baseurl = https://packages.vmware.com/packages/rhel7/x86_64/
    enabled = 1
    gpgcheck = 1

yum install -y open-vm-tools-deploypkg perl
systemctl restart vmtoolsd
```
## Generate and copy certificate from Terraform management VM
```sh
[root@linux-mgmt]# ssh-keygen
[root@linux-mgmt]# ssh-copy-id username@templateVM
```
## Cleanup
Clean history
```
history -c
```
Unconfigure VM
```
sys-unconfig
```

# Provisionning with DHCP
Use folder /terraform_centos7_vsphere_DHCP

Edit file terraform.tfvars example below
```
hostname = "test"
disk_size = "10"
dvs_vlan = "vlan5"
cpu = "2"
memory = "1024"

root_size_percentage = "16"
var_size_percentage = "74"
domain = "example.com"
datastore = "datastore1"
datacenter = "datacenter1"
cluster = "cluster1"
template = "template_centos7"
vsphere_vcenter = "vc01.example.com"
vsphere_user = "admin"
vsphere_password = "password"
vsphere_datacenter = "datacenter1"
vsphere_cluster = "cluster1"
```
In same folder execute
* terraform plan
* terraform apply

Done :)




