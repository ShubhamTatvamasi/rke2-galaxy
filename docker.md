# docker


Download image on the rke2 node:
```bash
sudo /var/lib/rancher/rke2/bin/ctr \
  --address /run/k3s/containerd/containerd.sock \
  -n k8s.io \
  images pull \
  docker.io/library/traefik:v3.7.4
```

---

Add your docker credentials to avoid `pull rate limit`:
```bash
sudo vim /etc/rancher/rke2/registries.yaml
```


```
configs:
  "docker.io":
    auth:
      username: YOUR_USERNAME
      password: YOUR_PASSWORD
```
> use `YOUR_TOKEN`

Restart rke2 server:
```bash
sudo systemctl restart rke2-server
```



