# Ansible Role: kubernetes_prepare

Prepares a system for Kubernetes deployment by configuring kernel modules, sysctl settings, installing Kubernetes packages, and setting up the kubelet service.

## Requirements

- systemd-based Linux distribution (Fedora, RHEL, CentOS)
- Ansible 2.9 or higher

## Role Variables

This role does not require any variables to be set. All configuration is applied with sensible defaults.

## What This Role Does

1. **Kernel Modules**: Configures `br_netfilter` and `overlay` modules to load on boot and ensures they are loaded
2. **Sysctl Settings**: Enables IP forwarding and bridge netfilter for iptables
3. **Zram**: Disables zram to prevent conflicts with Kubernetes
4. **Kubernetes Packages**: Installs:
   - containernetworking-plugins
   - helm
   - kubernetes1.34
   - kubernetes1.34-client
   - kubernetes1.34-kubeadm
5. **CNI Setup**: Creates `/opt/cni/bin` symlink to `/usr/libexec/cni`
6. **Kubelet Service**: Enables and starts kubelet
7. **Firewall**: Disables firewalld if present (Kubernetes manages its own networking)

## Example Playbook

```yaml
- hosts: k8s_nodes
  become: true
  roles:
    - role: kubernetes_prepare
```

## Sysctl Settings Applied

- `net.bridge.bridge-nf-call-iptables = 1`
- `net.bridge.bridge-nf-call-ip6tables = 1`
- `net.ipv4.ip_forward = 1`

## License

MIT

## Author Information

Created for preparing systems for Kubernetes cluster deployment.
