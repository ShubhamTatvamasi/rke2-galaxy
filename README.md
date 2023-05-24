# rke2-galaxy

Install RKE2 with Ansible Library:
```bash
ansible-galaxy install -f lablabs.rke2
```

Start a VM with Multipass:
```bash
multipass launch jammy \
  --name rke2 \
  --disk 100G \
  --memory 4G \
  --cpus 4
```

Access the shell:
```bash
multipass shell rke2
```

Update your public key:
```bash
vim ~/.ssh/authorized_keys
```

Update the IP address in `hosts.yml`:
```bash
export RKE2_IP=$(multipass info rke2 --format=json | jq -r '.info.rke2.ipv4[]')
yq e '.masters.hosts = env(RKE2_IP)' -i hosts.yml
```

Install RKE2:
```bash
ansible-playbook deploy-rke2.yml
```

Test kubeconfig:
```bash
export KUBECONFIG=/tmp/rke2.yaml

kubectl get nodes
```

### Uninstall

Delete Instance:
```bash
multipass delete --purge rke2
```
