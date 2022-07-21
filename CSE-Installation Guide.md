---
layout: post
autore: "Nabil Sato"
title: Install CSE Server
comments: false
category: blog
---

# CSE Prerequisites

## deploy photon OS 3 

Download and import photon3 OVF template and attacch it on the vCloud director management network (if you are using different network open the firewall ports between CSE server and vCloud for HTTPS and AMPQ.

![photon import](/Images/cse-installation/photon-import.png)


When the photon is imported, power on the VM and configure network interface and DNS.

(example)
```bash
cat > /etc/systemd/network/10-static-en.network << "EOF"
 
[Match]
Name=eth0
 
[Network]
Address=192.168.200.107/24
Gateway=192.168.200.254
DNS=192.168.200.101 192.168.200.102
Domains=yourdomain.local
EOF
```
Then restart network and DNS services:

```bash
chmod 644 /etc/systemd/network/10-static-en.network

systemctl restart systemd-networkd

systemdctl restart systemd-resolved

curl google.com
```
you need HTTP/HTTPS internet connectivity on this VM to download the required packages.



## Install CSE 

### Install requirements

At this point update the packages on the photon machine before install the CSE service:


```bash
tdnf --assumeyes update
```
*note: this step needs from 5 to 10 minutes to complete*


Now install the Python3 utilities (CSE dependencies)
```bash
tdnf --assumeyes install build-essential python3-devel python3-pip git
```

### create cse user and python virtual env

Create cse user with custom home directory:

```bash
mkdir -p /opt/vmware/cse
chmod 775 -R /opt
```
add new group and user named cse

```bash
groupadd cse
useradd cse -g cse -m -p PASSWORD -d /opt/vmware/cse
chown cse:cse -R /opt
```
run as CSE user:

```bash
su - cse
```
add variables to bash_profile

```bash
cat >> ~/.bash_profile << EOF
export CSE_CONFIG=/opt/vmware/cse/config/config.yaml
export CSE_CONFIG_PASSWORD=PASSWORD
source /opt/vmware/cse/python/bin/activate
EOF
```
*note: the variables will be setted at every login and the CSE config file will be decrypted with the "PASSWORD"*

## Install the Container Service Extension

To install the CSE software in the Python Virtual Environment please follow the commands below:

```bash
python3 -m venv /opt/vmware/cse/python
source /opt/vmware/cse/python/bin/activate
pip3 install container-service-extension==3.1.3
```
![cse-install](/Images/cse-installation/cse-installation.png)

*the string "container-service-extension==3.1.3" define the version of the cse server, in this case the version is 3.1.3*

Install the vcd-cli and create config dir .vcd.cli:

```bash
pip3 install --user vcd-cli
mkdir ~/.vcd-cli
```

Associate the CSE extension to VCD-CLI:

```bash
cat >  ~/.vcd-cli/profiles.yaml << EOF
extensions:
- container_service_extension.client.cse
EOF
```

Verify if the extension is configured correctly:

```bash
vcd cse version
```
>```
> CSE, Container Service Extension for VMware vCloud Director, version 3.1.3
>```

### create cse role in vcd

Use the following VCD-CLI command to log on to your VMware Cloud Director environment
```bash
vcd login vcloud.yourdomain.com system administrator -p PASSWORD
```

Create a new CSE Service Role on your VMware Cloud Director environment
```bash
cse create-service-role vcloud.yourdomain.com
```

Create a new CSE Service account on your VMware Cloud Director environment
```bash
vcd user create --enabled svc_cse PASSWORD "CSE Service Role"
```


## Configure Cloud director Catalog

deploy TKG clusters needs a templates upload from cse to a specific shared catalog in VCD,
for this we need to create a catalog in a ORG-VDC.

![cse-catalog](/Images/cse-installation/tkg-catalog.png)

thenn create a network in this org will be used in the next steps


**IMPORTANT: the deployed TKGm cluster needs internet connection, the customer needs to create a SNAT for internet connectivity on the org network defined for the cluster. Also DNS must be working weel for this network and the cluster needs public name resolution**  


## CSE config.yaml

In this file, you need to define your Cloud Director instance, vCenter Server, Storage Policy, VCD Organization, and more. Is the core config file and is compiled in clear and then will be encrypted with an encryption password.

first of all just create a config directory:

```bash
mkdir -p /opt/vmware/cse/config
```

then generate a sample decrypted config.yaml file:
```bash
cse sample > /opt/vmware/cse/config/config-not-encrypted.yaml
```
open the file in  editor and fill the information:

```yaml
mqtt:
  verify_ssl: true

vcd:
  host: vcd.vmware.com
  log: true
  password: my_secret_password
  port: 443
  username: administrator
  verify: true

vcs:
- name: vc1
  password: my_secret_password
  username: cse_user@vsphere.local
  verify: true
- name: vc2
  password: my_secret_password
  username: cse_user@vsphere.local
  verify: true

service:
  enforce_authorization: false
  legacy_mode: false
  log_wire: false
  no_vc_communication_mode: false
  processors: 15
  telemetry:
    enable: true

broker:
  catalog: cse
  ip_allocation_mode: pool
  network: my_network
  org: my_org
  remote_template_cookbook_url: https://raw.githubusercontent.com/vmware/container-service-extension-templates/master/template_v2.yaml
  storage_profile: '*'
  vdc: my_org_vdc

extra_options:
  my_key: my_value

# [Optional] Extra options section
#extra_options:
#  tkgm_http_proxy: [http proxy url with port]
#  tkgm_https_proxy: [https proxy url with port]
#  tkgm_no_proxy: [comma separated list of IP addresses]
#  cpi_version: "1.1.1"
#  csi_version: "1.2.0"
#  antrea_version: "0.11.3"
```

you need to change:

- MQTT verification (we use the embedded MQTT)
- VCD endpoint and define ssl verification (self-signed can be "false")
- remove vcs (vcenter) information if you are planning to use TKGm (in this guide we use TKGm)
- for validate the previous step we need to set  the no_vc_communication_mode to true
- in the broker section is important to set the ORG, ORG-VCD, ip allocation and important the catalog where the template are stored.

example of config file:

```yaml
mqtt:
  verify_ssl: false

vcd:
  host: vcloud.desotech.local
  log: true
  password: ********
  port: 443
  username: administrator
  verify: false


service:
  enforce_authorization: false
  legacy_mode: false
  log_wire: false
  no_vc_communication_mode: true
  processors: 15
  telemetry:
    enable: true

broker:
  catalog: CSE-TKGm-Catalog
  ip_allocation_mode: pool
  network: cse-network
  org: cse-org
  remote_template_cookbook_url: https://raw.githubusercontent.com/vmware/container-service-extension-templates/master/template_v2.yaml
  storage_profile: 'vSAN Default Storage Policy'
  vdc: CSE-Catalog-ORG


# [Optional] Extra options section
#extra_options:
#  tkgm_http_proxy: [http proxy url with port]
#  tkgm_https_proxy: [https proxy url with port]
#  tkgm_no_proxy: [comma separated list of IP addresses]
#  cpi_version: "1.1.1"
#  csi_version: "1.2.0"
#  antrea_version: "0.11.3"
```
*note: the extra options are optional and define the kubernetes components versions.*


when the file is correctly compiled we can encrypt it and use it to start cse. (needs to put an encryption password) 

```bash
cse encrypt /opt/vmware/cse/config/config-not-encrypted.conf --output /opt/vmware/cse/config/config.yaml
```

change permission:
```bash
chmod 600 /opt/vmware/cse/config/config.yaml
```
Check if the configuration is valid for cse:

```bash
cse check /opt/vmware/cse/config/config.yaml
```

### Create CSE enabled Service:

On the CSE server machine with root user create a Linux service Unit file:


- create a startup script:
  
```bash
cat > /opt/vmware/cse/cse.sh << EOF
#!/usr/bin/env bash
source /opt/vmware/cse/python/bin/activate
export CSE_CONFIG=/opt/vmware/cse/config/config.yaml
export CSE_CONFIG_PASSWORD=PASSWORD
cse run
EOF
```
Make cse.sh executable
```bash
chmod +x /opt/vmware/cse/cse.sh
```
  

  
# Setup cse.service unit file
```bash
cat > /etc/systemd/system/cse.service << EOF
[Unit]
Description=Container Service Extension for VMware Cloud Director
  
[Service]
ExecStart=/opt/vmware/cse/cse.sh
User=cse
WorkingDirectory=/opt/vmware/cse
Type=simple
Restart=always
  
[Install]
WantedBy=default.target
EOF
```

enable and start the service:

```bash
systemctl enable cse.service
systemctl start cse.service
  
systemctl status cse.service
```


### install CSE on Cloud Director:

on the cse server install CSE based on the config file

```bash
cse install -c /opt/vmware/cse/config/config.yaml
```

then go on the vcloud director customize portal, enable the "Container UI Plugin" and publish it to the KaaS tenants 

![cse-portal](/Images/cse-installation/customize%20portal.png)


![cse-publish-plugin](/Images/cse-installation/publish-cplugin.png)

![cse-to-tenants](/Images/cse-installation/tenant-publish.png)





### Permissions

There is another operation to perform for grant permission on the kubernetes cluster creation on the vCloud administration,
you need to publish the rights bundle for TKGm cluster creation:

go to Administration > Tenant Access Control > Rights Bundle, then select "vmware:tkgcluster Entitlement" and publish it to the KaaS tenants:


![cse-rights-bundle](/Images/cse-installation/rights-bundle.png)
 



Then assign this privileges to the Organization Administrator Global Role:

go to Administration > Tenant Access Control > Global Roles, then select Organization Administrator in the list and click edit,

scroll down to the OTHER and select all View and all Manage (check if there are only permission related to CSE service).

![cse-global-role](/Images/cse-installation/global-role.png)



**IMPORTANT: if you needs Native clusters or the permission not work please try to repeat this steps with "cse:nativeCluster Entitlement"**


## Import Templates

Add recent TKGm templates to your environment by running the following commands:

- Download .OVA template van my.vmware.com > ubuntu-2004-kube-v1.21.2+vmware.1-tkg.1-7832907791984498322.ova
- Upload to the /tmp folder of your vcd-cse01 server with WinSCP.
- Update the rights to 0644

then use cse import command to uploud the template on the vCloud Catalog (use this command in cse virtual env)

```bash
cse template import -F /tmp/ubuntu-2004-kube-v1.21.2+vmware.1-tkg.1-7832907791984498322.ova
```
*note: the import process of a single templates needs few minutes 5-10*


![cse-import](/Images/cse-installation/template-import.png)
![cse-global-role](/Images/cse-installation/catalog-template.png)




Every time, when you import a new templates, CSE service needs to be restarted for discovering new changes.

```bash
systemctl restart cse.service
  
systemctl status cse.service
```

## Use KaaS

At this point is possible to use  Container Service Extension for deploy Kubernetes Clusters:

Open tenant portal > More > Kubernetes Container Clusters > New

![cse-import](/Images/cse-installation/container-cluster.png)

![cse-import](/Images/cse-installation/TKG-Cluster.png)

![cse-import](/Images/cse-installation/cluster-finish.png)



Complete the cluster creation wizard and in a few minutes you can access to your cluster witha a downloadble kubeconfig file.

![cse-import](/Images/cse-installation/kubeconfig.png)

Enjoy your KaaS Platform.
---
Nabil Sato

Desotech srl
