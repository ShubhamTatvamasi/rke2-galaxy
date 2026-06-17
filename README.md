# rke2-galaxy

https://github.com/lablabs/ansible-role-rke2 \
https://github.com/rancher/rke2

Install RKE2 with Ansible Library:
```bash
ansible-galaxy install -f lablabs.rke2
```

Install RKE2:
```bash
ansible-playbook deploy-rke2.yml
```

Get kubeconfig file from node:
```bash
sudo cat /etc/rancher/rke2/rke2.yaml
```
> Change the IP
