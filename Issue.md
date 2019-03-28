Vagrant up
---

## Commands

pip install  said this:

```
sudo -HE pip install -r requirements.txt

    Requirement already satisfied: ansible!=2.7.0,>=2.5.0 in /usr/local/lib/python2.7/site-packages (from -r requirements.txt (line 1)) (2.7.9)
    Requirement already satisfied: jinja2>=2.9.6 in /usr/local/lib/python2.7/site-packages (from -r requirements.txt (line 2)) (2.10)
    Requirement already satisfied: netaddr in /usr/local/lib/python2.7/site-packages (from -r requirements.txt (line 3)) (0.7.19)
    Requirement already satisfied: pbr>=1.6 in /usr/local/lib/python2.7/site-packages (from -r requirements.txt (line 4)) (5.1.3)
    Requirement already satisfied: hvac in /usr/local/lib/python2.7/site-packages (from -r requirements.txt (line 5)) (0.7.2)
    Requirement already satisfied: PyYAML in /usr/local/lib/python2.7/site-packages (from ansible!=2.7.0,>=2.5.0->-r requirements.txt (line 1)) (3.13)
    Requirement already satisfied: paramiko in /usr/local/lib/python2.7/site-packages (from ansible!=2.7.0,>=2.5.0->-r requirements.txt (line 1)) (2.4.2)
    Requirement already satisfied: cryptography in /usr/local/lib/python2.7/site-packages (from ansible!=2.7.0,>=2.5.0->-r requirements.txt (line 1)) (2.5)
    Requirement already satisfied: setuptools in /usr/local/lib/python2.7/site-packages (from ansible!=2.7.0,>=2.5.0->-r requirements.txt (line 1)) (40.2.0)
    Requirement already satisfied: MarkupSafe>=0.23 in /usr/local/lib/python2.7/site-packages (from jinja2>=2.9.6->-r requirements.txt (line 2)) (1.1.0)
    Requirement already satisfied: requests>=2.21.0 in /usr/local/lib/python2.7/site-packages (from hvac->-r requirements.txt (line 5)) (2.21.0)
    Requirement already satisfied: pyasn1>=0.1.7 in /usr/local/lib/python2.7/site-packages (from paramiko->ansible!=2.7.0,>=2.5.0->-r requirements.txt (line 1)) (0.4.5)
    Requirement already satisfied: bcrypt>=3.1.3 in /usr/local/lib/python2.7/site-packages (from paramiko->ansible!=2.7.0,>=2.5.0->-r requirements.txt (line 1)) (3.1.6)
    Requirement already satisfied: pynacl>=1.0.1 in /usr/local/lib/python2.7/site-packages (from paramiko->ansible!=2.7.0,>=2.5.0->-r requirements.txt (line 1)) (1.3.0)
    Requirement already satisfied: enum34; python_version < "3" in /usr/local/lib/python2.7/site-packages (from cryptography->ansible!=2.7.0,>=2.5.0->-r requirements.txt (line 1)) (1.1.6)
    Requirement already satisfied: cffi!=1.11.3,>=1.8 in /usr/local/lib/python2.7/site-packages (from cryptography->ansible!=2.7.0,>=2.5.0->-r requirements.txt (line 1)) (1.11.5)
    Requirement already satisfied: six>=1.4.1 in /usr/local/lib/python2.7/site-packages (from cryptography->ansible!=2.7.0,>=2.5.0->-r requirements.txt (line 1)) (1.11.0)
    Requirement already satisfied: asn1crypto>=0.21.0 in /usr/local/lib/python2.7/site-packages (from cryptography->ansible!=2.7.0,>=2.5.0->-r requirements.txt (line 1)) (0.24.0)
    Requirement already satisfied: ipaddress; python_version < "3" in /usr/local/lib/python2.7/site-packages (from cryptography->ansible!=2.7.0,>=2.5.0->-r requirements.txt (line 1)) (1.0.22)
    Requirement already satisfied: certifi>=2017.4.17 in /usr/local/lib/python2.7/site-packages (from requests>=2.21.0->hvac->-r requirements.txt (line 5)) (2019.3.9)
    Requirement already satisfied: chardet<3.1.0,>=3.0.2 in /usr/local/lib/python2.7/site-packages (from requests>=2.21.0->hvac->-r requirements.txt (line 5)) (3.0.4)
    Requirement already satisfied: idna<2.9,>=2.5 in /usr/local/lib/python2.7/site-packages (from requests>=2.21.0->hvac->-r requirements.txt (line 5)) (2.8)
    Requirement already satisfied: urllib3<1.25,>=1.21.1 in /usr/local/lib/python2.7/site-packages (from requests>=2.21.0->hvac->-r requirements.txt (line 5)) (1.24.1)
    Requirement already satisfied: pycparser in /usr/local/lib/python2.7/site-packages (from cffi!=1.11.3,>=1.8->cryptography->ansible!=2.7.0,>=2.5.0->-r requirements.txt (line 1)) (2.19)
```

```
cp -rfp inventory/sample inventory/mycluster
declare -a IPS=(10.10.1.3 10.10.1.4 10.10.1.5)
CONFIG_FILE=inventory/mycluster/hosts.ini python3 contrib/inventory_builder/inventory.py ${IPS[@]}
```
Outputs:

```
DEBUG: Adding group all
DEBUG: Adding group kube-master
DEBUG: Adding group kube-node
DEBUG: Adding group etcd
DEBUG: Adding group k8s-cluster:children
DEBUG: Adding group calico-rr
DEBUG: Adding group vault
DEBUG: adding host node1 to group all
DEBUG: adding host node2 to group all
DEBUG: adding host node3 to group all
DEBUG: adding host kube-node to group k8s-cluster:children
DEBUG: adding host kube-master to group k8s-cluster:children
DEBUG: adding host node1 to group etcd
DEBUG: adding host node1 to group vault
DEBUG: adding host node2 to group etcd
DEBUG: adding host node2 to group vault
DEBUG: adding host node3 to group etcd
DEBUG: adding host node3 to group vault
DEBUG: adding host node1 to group kube-master
DEBUG: adding host node2 to group kube-master
DEBUG: adding host node1 to group kube-node
DEBUG: adding host node2 to group kube-node
DEBUG: adding host node3 to group kube-node
```

Options
```
mkdir inventory.orig
mv inventory/{sample,local}/ inventory.orig/
```

# Modifications to Vagrantfile

```
diff --git a/Vagrantfile b/Vagrantfile
index 9e587c79..9f87f61e 100644
--- a/Vagrantfile
+++ b/Vagrantfile
@@ -61,7 +61,7 @@ end
 
 $box = SUPPORTED_OS[$os][:box]
 # if $inventory is not set, try to use example
-$inventory = "inventory/sample" if ! $inventory
+$inventory = "inventory/mycluster/hosts.ini" if ! $inventory
 $inventory = File.absolute_path($inventory, File.dirname(__FILE__))
 
 # if $inventory has a hosts.ini file use it, otherwise copy over
@@ -168,7 +168,7 @@ Vagrant.configure("2") do |config|
         "kube_network_plugin": $network_plugin,
         "kube_network_plugin_multus": $multi_networking,
         "docker_keepcache": "1",
-        "download_run_once": "True",
+        "download_run_once": "False",
         "download_localhost": "False"
       }

```

Output
---
I prefer some more readable output

```
diff --git a/ansible.cfg b/ansible.cfg
index bed2c4ae..11a9a4fe 100644
--- a/ansible.cfg
+++ b/ansible.cfg
@@ -9,11 +9,17 @@ host_key_checking=False
 gathering = smart
 fact_caching = jsonfile
 fact_caching_connection = /tmp
-stdout_callback = skippy
+# stdout_callback = skippy
 library = ./library
 callback_whitelist = profile_tasks
 roles_path = roles:$VIRTUAL_ENV/usr/local/share/kubespray/roles:$VIRTUAL_ENV/usr/local/share/ansible/roles:/usr/share/kubespray/roles
 deprecation_warnings=False
 inventory_ignore_extensions = ~, .orig, .bak, .ini, .cfg, .retry, .pyc, .pyo, .creds
+
+# Use the YAML callback plugin.
+stdout_callback = yaml
+# Use the stdout_callback when running ad-hoc commands.
+bin_ansible_callbacks = True
+
 [inventory]
 ignore_patterns = artifacts, credentials
```




run
```
vagrant up
vagrant up --provision
```

Get a copy of the kubeconfig

```
[0](v2.8.3-tag):kubespray $ vagrant ssh k8s-1
WARNING: /vagrant/util/which.rb:37: Insecure world writable dir /opt in PATH, mode 040777
Last login: Wed Mar 27 20:19:21 2019 from 10.0.2.2
vagrant@k8s-1:~$ sudo cat /root/.kube/config

# OR

vagrant ssh k8s-1 -- sudo cat /root/.kube/config > kubeconfig

```

Run with this new config

```
KUBECONFIG=${PWD}/kubeconfig kubectl get po

No resources found.
[0](v2.8.3-tag):kubespray $ KUBECONFIG=${PWD}/kubeconfig kubectl get no
NAME    STATUS   ROLES         AGE     VERSION
k8s-1   Ready    master,node   6h26m   v1.12.5
k8s-2   Ready    master,node   6h25m   v1.12.5
k8s-3   Ready    node          6h24m   v1.12.5

```
