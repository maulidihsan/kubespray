## Prerequisite
Open port:
- 10250/tcp (kubelet API)
- 10251-10252/tcp (kube-scheduler, kube-controller-manager *only master node*)
- 6443/tcp (kube-apiserver)
- 2379-2380/tcp (etcd)
- 179/tcp (calico)

Disable SELinux
Set Hostname in inventory/kubeklaster/hosts.ini

#### Usage

```ShellSession
# Install dependencies from ``requirements.txt``
sudo pip install -r requirements.txt

# Review and change parameters under ``inventory/kubeklaster/group_vars``
cat inventory/kubeklaster/group_vars/all/all.yml
cat inventory/kubeklaster/group_vars/k8s-cluster/k8s-cluster.yml

# Deploy Kubespray with Ansible Playbook - run the playbook as root
ansible-playbook -i inventory/kubeklaster/hosts.ini -k -u user --become --become-user=root cluster.yml
```

## Network Plugins

You can choose between 10 network plugins. (default: `calico`, except Vagrant uses `flannel`)

- [flannel](docs/flannel.md): gre/vxlan (layer 2) networking.

- [Calico](https://docs.projectcalico.org/latest/introduction/) (default) is a networking and network policy provider. Calico supports a flexible set of networking options
    designed to give you the most efficient networking across a range of situations, including non-overlay
    and overlay networks, with or without BGP. Calico uses the same engine to enforce network policy for hosts,
    pods, and (if using Istio and Envoy) applications at the service mesh layer.

- [canal](https://github.com/projectcalico/canal): a composition of calico and flannel plugins.

- [cilium](http://docs.cilium.io/en/latest/): layer 3/4 networking (as well as layer 7 to protect and secure application protocols), supports dynamic insertion of BPF bytecode into the Linux kernel to implement security services, networking and visibility logic.

- [contiv](docs/contiv.md): supports vlan, vxlan, bgp and Cisco SDN networking. This plugin is able to
    apply firewall policies, segregate containers in multiple network and bridging pods onto physical networks.

- [weave](docs/weave.md): Weave is a lightweight container overlay network that doesn't require an external K/V database cluster.
    (Please refer to `weave` [troubleshooting documentation](https://www.weave.works/docs/net/latest/troubleshooting/)).

- [kube-ovn](docs/kube-ovn.md): Kube-OVN integrates the OVN-based Network Virtualization with Kubernetes. It offers an advanced Container Network Fabric for Enterprises.

- [kube-router](docs/kube-router.md): Kube-router is a L3 CNI for Kubernetes networking aiming to provide operational
    simplicity and high performance: it uses IPVS to provide Kube Services Proxy (if setup to replace kube-proxy),
    iptables for network policies, and BGP for ods L3 networking (with optionally BGP peering with out-of-cluster BGP peers).
    It can also optionally advertise routes to Kubernetes cluster Pods CIDRs, ClusterIPs, ExternalIPs and LoadBalancerIPs.

- [macvlan](docs/macvlan.md): Macvlan is a Linux network driver. Pods have their own unique Mac and Ip address, connected directly the physical (layer 2) network.

- [multus](docs/multus.md): Multus is a meta CNI plugin that provides multiple network interface support to pods. For each interface Multus delegates CNI calls to secondary CNI plugins such as Calico, macvlan, etc.

The choice is defined with the variable `kube_network_plugin`. There is also an
option to leverage built-in cloud provider networking instead.
See also [Network checker](docs/netcheck.md).
