# ansible-proxmox-vm-deploy

Deploy a VM to Proxmox with ansible.

This ansible roles assumes the usage of a Template that is CloudInit "ready".

This role does the following things:

1. Create new VM in proxmox from a template.

2. Allocate the supplied IP address in Netbox (IPAM / DCIM).

3. Run the puppet agent on the new VM and sign the certificate request on the puppet master.

```
ansible-playbook -vv -i hosts/proxmox-local roles/proxmox/tasks/vm_deploy.yml site.yml \
       --tags vm_deploy -e "PVE_API=${PVE_API_HOST} PVE_USER=${PVE_USER} PVE_PASS=${PVE_PASS}" \
       -e "VM_NAME=${VMHOSTNAME} PVE_TEMPLATE_NAME=${TEMPLATE} PVE_SRC_NODE=${PVE_SRC_NODE} PVE_DEST_NODE=${PVE_DEST_NODE}" \
       -e "VM_CPU=${cpu} VM_MEM=${mem_mb} VM_BRIDGE=${VM_BRIDGE} VM_VLAN=${vlan} VM_STORAGE=${storage} VM_FORMAT=${format}" \
       -e "VM_POOL=${pool} vm_ip=${IPADDRESS} vm_cidr=${cidr} vm_gateway=${gateway} vm_dns=${DNS}" \
       -e "puppetmaster=${PUPPET_MASTER} second_disk_size=${second_disk_size} path_to_private_key=${KEY} vm_desc=${owner}"
```
