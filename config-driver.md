
### 1 构建数据源:
```bash
mkdir -p config-2/openstack/2012-08-10
ln -sf 2012-08-10 latest
mkdir -p config-2/openstack/content
touch config-2/openstack/2012-08-10/meta_data.json
touch config-2/openstack/2012-08-10/user_data
touch config-2/openstack/content/0000
```

*meta_data.json:*
```bash
{
    "network_config" : {
        "content_path" : "/content/0000",
        "path" : "/etc/network/interfaces"
    },

    "uuid": "6c559874-f3ce-48b6-b377-c99d878edbac"
}
```
*content/0000:*
```bash
auto eth0
iface eth0 inet static
address 192.168.1.33
netmask 255.255.255.0
gateway 192.168.1.1
```

*user_data:*
```bash
#cloud-config
password: redhat
chpasswd: { expire: False }
ssh_pwauth: True
ssh_authorized_keys:
 - ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQC6AqKvxzQWaJZKwg0I0+nMSRCMcBbmsrTkIzMFzIIUAKhnuzOVB+P8dUXfZXI+UAruEQybmIYWznQMB8a3kbWdTyjeNmxoTS/zUMLHlXMxA11YcO78KA/28ptW6LYgASIQzRG6wyklHUorloxwJPMC5x99A0xwWQVr/uwC+0milTh0WY+cI3mGyTBVPzRMcG2ElWsx4ppOGbubrL65ZocTeRNQPKibVKruPz8apWdX74blyAWR5VkxKd75/kALr5adGysfbztiZ44RcB95hUVrjM2R0Kv+clbA6ThE5AVp26/SVcG8FvVaDzZHes/BPCBplZDmr7zwTSe7AtmlrW4J root@localhost.localdomain
final_message: "The system is finally up, after $UPTIME seconds"

users:
 - default
 - name: foobar
   lock-passwd: false
   sudo: ALL=(ALL) NOPASSWD:ALL
   plain_text_passwd: 'redhat'
```


### 2 
```bash
mkdir -p /tmp/new-drive/openstack/latest
cp user_data /tmp/new-drive/openstack/latest/user_data
mkisofs -R -V config-2 -o configdrive.iso /tmp/new-drive
rm -r /tmp/new-drive
gzip configdrive.iso
base64 configdrive.iso.gz
```