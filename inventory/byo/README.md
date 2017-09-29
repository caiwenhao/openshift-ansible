# OpenShift Ansible 安装

github开源项目使用最佳实践

> 参考 https://github.com/caiwenhao/kubespray/blob/master/docs/integration.md



环境准备:

```
yum install git ansible pyopenssl python-lxml
easy_install jinja2
cd /data/
git clone https://github.com/caiwenhao/openshift-ansible.git
cd openshift-ansible
git checkout -b lifesense origin/lifesense
```

1. 修改远程用户名
```
ansible_ssh_user=lifesense
```

2.  修改版本为3.6 
```
openshift_release=v3.6
```

3. 禁止磁盘空间和内存检查
```
openshift_disable_check=disk_availability,memory_availability,docker_image_availability

```

4. 安装docker

> openshift_version 注释when判断 

```
openshift.common.is_containerized=true
openshift_docker_options="-l warn --ipv6=false --log-driver=journald --registry-mirror=http://b377ad59.m.daocloud.io -s overlay2"
```
创建软链接
ln -s /data/docker_root /var/lib/docker


5. /sbin/service iptables restart

```
新增一空行 /etc/sysconfig/iptables
```

6. ucloud上配置内网负债均衡器

```
vim /etc/sysconfig/network-scripts/ifcfg-lo:1
DEVICE=lo:1
IPADDR=10.10.100.49
NETMASK=255.255.255.255
```
ifup lo:1


7. NetworkManager

/roles/openshift_node_dnsmasq/tasks/main.yml
```
- name: Install NetworkManager
  package: name=NetworkManager state=installed
  when: not openshift.common.is_atomic | bool

- name: Enable NetworkManager
  systemd:
    name: NetworkManager
    enabled: yes
    state: started
```

openshift_node : restart node
```
/etc/origin/node/resolv.conf
```

```
- name: copy /etc/resolv.conf
  copy: src=/etc/resolv.conf dest=/etc/origin/node/resolv.conf
```


8. 
'keytool' is unavailable. Please install java-1.8.0-openjdk-headless on the control node

```
yum install java-1.8.0-openjdk-headless
```