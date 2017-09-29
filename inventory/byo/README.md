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

3. 禁止磁盘空间检查
```
openshift_disable_check=disk_availability
```