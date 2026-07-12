# Sysctl

## 1. Sysctl tuning

Create:

```bash
sudo vim /etc/sysctl.d/90-rke2.conf
```

Add:

```conf
# Kubernetes networking
net.bridge.bridge-nf-call-iptables = 1
net.bridge.bridge-nf-call-ip6tables = 1
net.ipv4.ip_forward = 1

# TCP/network tuning
net.ipv4.tcp_tw_reuse = 1
net.ipv4.tcp_fin_timeout = 15
net.core.somaxconn = 65535
net.core.netdev_max_backlog = 250000

# Conntrack
net.netfilter.nf_conntrack_max = 1048576

# Inotify
fs.inotify.max_user_watches = 1048576
fs.inotify.max_user_instances = 2048
fs.inotify.max_queued_events = 32768

# File handles
fs.file-max = 2097152

# VM tuning
vm.swappiness = 0
vm.max_map_count = 262144
vm.overcommit_memory = 1

# PID limit
kernel.pid_max = 4194304
```

Apply:

```bash
sudo sysctl --system
```

---

## 2. Kernel modules

Create:

```bash
sudo vim /etc/modules-load.d/rke2.conf
```

Add:

```text id="mfv2yo"
overlay
br_netfilter
nf_conntrack
```

Load now:

```bash
sudo modprobe overlay
sudo modprobe br_netfilter
sudo modprobe nf_conntrack
```

---

## 3. Disable swap

Temporary:

```bash
sudo swapoff -a
```

Permanent:

```bash
sudo vim /etc/fstab
```

Comment swap line:

```text
#/swap.img none swap sw 0 0
```

Verify:

```bash
free -h
```

---

## 4. File descriptor limits

Create:

```bash
sudo vim /etc/security/limits.d/90-rke2.conf
```

Add:

```text
* soft nofile 1048576
* hard nofile 1048576
* soft nproc 1048576
* hard nproc 1048576

root soft nofile 1048576
root hard nofile 1048576
root soft nproc 1048576
root hard nproc 1048576
```

---

## 5. Systemd limits

Create:

```bash
sudo mkdir -p /etc/systemd/system.conf.d
sudo vim /etc/systemd/system.conf.d/limits.conf
```

Add:

```ini
[Manager]
DefaultLimitNOFILE=1048576
DefaultLimitNPROC=1048576
```

Reload:

```bash
sudo systemctl daemon-reexec
```

---

## 6. Install time sync

```bash
sudo apt update
sudo apt install chrony -y
```

Enable:

```bash
sudo systemctl enable --now chrony
```

Check:

```bash
chronyc tracking
```

---

## 7. SSD/NVMe tuning

Create:

```bash
sudo vim /etc/sysctl.d/99-ssd.conf
```

Add:

```conf
vm.dirty_ratio = 15
vm.dirty_background_ratio = 5
```

Apply:

```bash
sudo sysctl --system
```

---

## 8. Reboot

```bash
sudo reboot
```

---

## 9. Verify

```bash
sysctl fs.inotify.max_user_watches
sysctl net.ipv4.ip_forward
sysctl vm.max_map_count
ulimit -n
```

Expected:

```text
1048576
1
262144
1048576
```
