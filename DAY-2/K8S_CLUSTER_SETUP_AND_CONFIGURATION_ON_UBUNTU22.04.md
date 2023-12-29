**FOR MASTER**

sudo su -

hostnamectl set-hostname node

bash

cat /etc/hosts

<PRIVATE_IP_OF_THE_SERVER> node

sudo apt-get update && apt-get upgrade -y

Load the Kernel modules on all the nodes

Modprobe is a command-line-based utility in Linux based operating system that helps in removing and adding modules from the kernel.
This utility was written by Rusty Russel and is used to add a loadable module into the Linux kernel or remove a module from the Linux kernel.

sudo tee /etc/modules-load.d/containerd.conf <<EOF

overlay

br_netfilter

EOF

tee command reads the standard input and writes it to both the standard output and one or more files.


Modprobe is a command-line-based utility in Linux based operating system that helps in removing and adding modules from the kernel.
This utility was written by Rusty Russel and is used to add a loadable module into the Linux kernel or remove a module from the Linux kernel.

sudo modprobe overlay

sudo modprobe br_netfilter

########################################################################################################

Set the following Kernel params for K8s

The configuration parameter net.bridge.bridge-nf-call-ip6tables is related to Linux networking and is used to control whether IPv6 traffic is passed through the iptables framework. Here's a breakdown of its usage:

net.bridge.bridge-nf-call-ip6tables: This parameter is specific to bridged networking in Linux. It determines whether IPv6 packets that pass through a network bridge should be processed by the ip6tables firewall rules.

Values:

0: Disables the processing of IPv6 packets by ip6tables for bridged traffic.

1: Enables the processing of IPv6 packets by ip6tables for bridged traffic.

Usage:

If you set net.bridge.bridge-nf-call-ip6tables to 1, it means that IPv6 packets passing through the network bridge will be subject to the ip6tables firewall rules. This can be useful for applying firewall filtering and other security measures to IPv6 traffic in a bridged network environment.

If you set it to 0, it disables the processing of IPv6 packets by ip6tables for bridged traffic. This might be desirable in certain scenarios where you want to bypass IPv6 filtering for bridged packets.
The net.bridge.bridge-nf-call-iptables parameter, similar to its counterpart for IPv6 (net.bridge.bridge-nf-call-ip6tables), is a Linux kernel parameter that controls whether IPv4 traffic passing through a network bridge should be processed by the iptables firewall rules.

Here's a breakdown of its usage:

net.bridge.bridge-nf-call-iptables: This parameter is specific to bridged networking in Linux. It determines whether IPv4 packets that pass through a network bridge should be processed by the iptables firewall rules.

Values:

0: Disables the processing of IPv4 packets by iptables for bridged traffic.

1: Enables the processing of IPv4 packets by iptables for bridged traffic.

Usage:

If you set net.bridge.bridge-nf-call-iptables to 1, it means that IPv4 packets passing through the network bridge will be subject to the iptables firewall rules. This allows you to apply firewall filtering and other security measures to IPv4 traffic in a bridged network environment.

If you set it to 0, it disables the processing of IPv4 packets by iptables for bridged traffic. This might be desirable in certain scenarios where you want to bypass IPv4 filtering for bridged packets.

The net.ipv4.ip_forward parameter in Linux is a kernel parameter that determines whether the kernel should forward IPv4 packets from one network interface to another. Here's an explanation of its usage:

net.ipv4.ip_forward: This parameter controls IP packet forwarding for IPv4. When set to:

0: IP forwarding is disabled. The kernel will not forward packets between network interfaces.

1: IP forwarding is enabled. The kernel will forward packets between network interfaces.

Usage:

Enabling IP forwarding is necessary when a Linux system acts as a router or gateway between different network segments. In such scenarios, the system needs to forward packets from one network to another.

For example, if your Linux machine has two network interfaces, one connected to the internet and the other to a local network, and net.ipv4.ip_forward is set to 1, the machine will forward packets from the local network to the internet and vice versa.

sudo tee /etc/sysctl.d/kubernetes.conf <<EOF

net.bridge.bridge-nf-call-ip6tables = 1

net.bridge.bridge-nf-call-iptables = 1

net.ipv4.ip_forward = 1

EOF

########################################################################################################

Reload the system changes

sudo sysctl --system

########################################################################################################

Install containerd 

sudo apt install -y curl gnupg2 software-properties-common apt-transport-https ca-certificates

sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmour -o /etc/apt/trusted.gpg.d/docker.gpg

sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"

sudo apt update

sudo apt install -y containerd.io

#####################################################################################################

Configure containerd using systemd as cgroup

containerd config default | sudo tee /etc/containerd/config.toml >/dev/null 2>&1

The sed command, which stands for stream editor, is a powerful text-processing tool available in Unix and Linux environments. It is commonly used for performing text transformations on input streams (a file or input from a pipeline) and is especially useful for tasks like text substitution, deletion, and insertion.

sudo sed -i 's/SystemdCgroup \= false/SystemdCgroup \= true/g' /etc/containerd/config.toml

sudo systemctl restart containerd

sudo systemctl enable containerd

###################################################################################################

Add apt repository for k8s

curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -

sudo apt-add-repository "deb http://apt.kubernetes.io/ kubernetes-xenial main"

sudo apt update

sudo apt install -y kubelet kubeadm kubectl

sudo apt-mark hold kubelet kubeadm kubectl

kubeadm init 

##################################################################################################

Install Pod Network addon:

curl https://raw.githubusercontent.com/projectcalico/calico/v3.25.1/manifests/calico.yaml -O

**NODE**

sudo su -

hostnamectl set-hostname node

bash

cat /etc/hosts

<PRIVATE_IP_OF_THE_SERVER> node

sudo apt-get update && apt-get upgrade -y

Load the Kernel modules on all the nodes

Modprobe is a command-line-based utility in Linux based operating system that helps in removing and adding modules from the kernel.
This utility was written by Rusty Russel and is used to add a loadable module into the Linux kernel or remove a module from the Linux kernel.

sudo tee /etc/modules-load.d/containerd.conf <<EOF

overlay

br_netfilter

EOF

tee command reads the standard input and writes it to both the standard output and one or more files.


Modprobe is a command-line-based utility in Linux based operating system that helps in removing and adding modules from the kernel.
This utility was written by Rusty Russel and is used to add a loadable module into the Linux kernel or remove a module from the Linux kernel.

sudo modprobe overlay

sudo modprobe br_netfilter

########################################################################################################

Set the following Kernel params for K8s

The configuration parameter net.bridge.bridge-nf-call-ip6tables is related to Linux networking and is used to control whether IPv6 traffic is passed through the iptables framework. Here's a breakdown of its usage:

net.bridge.bridge-nf-call-ip6tables: This parameter is specific to bridged networking in Linux. It determines whether IPv6 packets that pass through a network bridge should be processed by the ip6tables firewall rules.

Values:

0: Disables the processing of IPv6 packets by ip6tables for bridged traffic.

1: Enables the processing of IPv6 packets by ip6tables for bridged traffic.

Usage:

If you set net.bridge.bridge-nf-call-ip6tables to 1, it means that IPv6 packets passing through the network bridge will be subject to the ip6tables firewall rules. This can be useful for applying firewall filtering and other security measures to IPv6 traffic in a bridged network environment.

If you set it to 0, it disables the processing of IPv6 packets by ip6tables for bridged traffic. This might be desirable in certain scenarios where you want to bypass IPv6 filtering for bridged packets.
The net.bridge.bridge-nf-call-iptables parameter, similar to its counterpart for IPv6 (net.bridge.bridge-nf-call-ip6tables), is a Linux kernel parameter that controls whether IPv4 traffic passing through a network bridge should be processed by the iptables firewall rules.

Here's a breakdown of its usage:

net.bridge.bridge-nf-call-iptables: This parameter is specific to bridged networking in Linux. It determines whether IPv4 packets that pass through a network bridge should be processed by the iptables firewall rules.

Values:

0: Disables the processing of IPv4 packets by iptables for bridged traffic.

1: Enables the processing of IPv4 packets by iptables for bridged traffic.

Usage:

If you set net.bridge.bridge-nf-call-iptables to 1, it means that IPv4 packets passing through the network bridge will be subject to the iptables firewall rules. This allows you to apply firewall filtering and other security measures to IPv4 traffic in a bridged network environment.

If you set it to 0, it disables the processing of IPv4 packets by iptables for bridged traffic. This might be desirable in certain scenarios where you want to bypass IPv4 filtering for bridged packets.

The net.ipv4.ip_forward parameter in Linux is a kernel parameter that determines whether the kernel should forward IPv4 packets from one network interface to another. Here's an explanation of its usage:

net.ipv4.ip_forward: This parameter controls IP packet forwarding for IPv4. When set to:

0: IP forwarding is disabled. The kernel will not forward packets between network interfaces.

1: IP forwarding is enabled. The kernel will forward packets between network interfaces.

Usage:

Enabling IP forwarding is necessary when a Linux system acts as a router or gateway between different network segments. In such scenarios, the system needs to forward packets from one network to another.

For example, if your Linux machine has two network interfaces, one connected to the internet and the other to a local network, and net.ipv4.ip_forward is set to 1, the machine will forward packets from the local network to the internet and vice versa.

sudo tee /etc/sysctl.d/kubernetes.conf <<EOF

net.bridge.bridge-nf-call-ip6tables = 1

net.bridge.bridge-nf-call-iptables = 1

net.ipv4.ip_forward = 1

EOF

########################################################################################################

Reload the system changes

sudo sysctl --system

########################################################################################################

Install containerd 

sudo apt install -y curl gnupg2 software-properties-common apt-transport-https ca-certificates

sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmour -o /etc/apt/trusted.gpg.d/docker.gpg

sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"

sudo apt update

sudo apt install -y containerd.io

#####################################################################################################

Configure containerd using systemd as cgroup

containerd config default | sudo tee /etc/containerd/config.toml >/dev/null 2>&1

The sed command, which stands for stream editor, is a powerful text-processing tool available in Unix and Linux environments. It is commonly used for performing text transformations on input streams (a file or input from a pipeline) and is especially useful for tasks like text substitution, deletion, and insertion.

sudo sed -i 's/SystemdCgroup \= false/SystemdCgroup \= true/g' /etc/containerd/config.toml

sudo systemctl restart containerd

sudo systemctl enable containerd

###################################################################################################

Add apt repository for k8s

curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -

sudo apt-add-repository "deb http://apt.kubernetes.io/ kubernetes-xenial main"

sudo apt update

sudo apt install -y kubelet kubeadm kubectl

sudo apt-mark hold kubelet kubeadm kubectl

##################################################################################################

systemctl restart containerd.service

systemctl restart kubelet.service

kubeadm join 172.31.30.204:6443 --token 3wlx43.ywbkejz8i6ykqaq5         --discovery-token-ca-cert-hash sha256:61efea6ac21c569738edf5bbdcda5a13ef4783a52c3f8d8226313638ced0f0e3
