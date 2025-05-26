# RKE2 Kubernetes Cluster Automation

A robust automation project for deploying RKE2 (Rancher Kubernetes Engine 2) clusters on local virtual machines using Multipass, cloud-init, and Ansible.

## 🚀 Overview

This project automates the deployment of a complete Kubernetes cluster using RKE2, with one master node and multiple worker nodes. It utilizes:

- **Multipass**: For lightweight VM management on macOS/Linux
- **cloud-init**: For initial VM configuration and setup
- **Ansible**: For orchestrating the RKE2 installation and configuration
- **Make**: For simplifying common operations

## 📋 Prerequisites

The project will check and install these dependencies if needed:
- Multipass
- Ansible
- yq
- jq

## 📁 Project Structure

```
├── inventory.yml           # Ansible inventory for cluster nodes
├── Makefile                # Automation commands
├── playbook.yml            # Main Ansible playbook
├── README.md               # This documentation
├── vms.yml                 # VM specifications
├── cloud-init/             # Initial node configurations
│   ├── master-01.yaml      # Master node configuration
│   ├── worker-01.yaml      # Worker node configurations
│   ├── worker-02.yaml      # Worker node configurations
│   └── worker-03.yaml      # Worker node configurations
└── roles/                  # Ansible roles
    ├── rke2-master/        # Master node setup
    └── rke2-worker/        # Worker node setup
```

## 🔧 Cluster Configuration

- **Master Node**: 
  - 4 CPUs, 4GB RAM, 20GB storage
  - Hosts the Kubernetes control plane
  - Configured with proper TLS SAN settings
  
- **Worker Nodes (3)**:
  - Each with 2 CPUs, 2GB RAM, 15GB storage
  - Run containerized applications
  - Automatically join the cluster

## 🛠️ Usage

### Setting Up the Cluster

```bash
# Deploy the complete cluster
make all

# Check VM status
make status

# Access a VM shell
make shell
```

### Managing the Cluster

```bash
# Update the inventory with current IPs
make update-inventory

# Run Ansible playbook separately
make run-ansible

# Connect to master node via SSH
make ssh-master

# Connect to a worker node via SSH (interactive selection)
make ssh-worker

# Destroy all VMs
make destroy

# Purge deleted VMs
make purge
```

## 📊 Cluster Details

- Uses SSH key authentication for all nodes
- Automatically configures networking between nodes
- Sets up kubeconfig for easy cluster management
- Configures proper node labels for workload distribution
- Performs automatic node health checks after deployment
- Displays cluster status after setup completes

## 🔒 Security Features

- Swap disabled on all nodes for Kubernetes compatibility
- User 'rke2' with sudo privileges for management
- SSH key-based authentication

## 💡 How It Works

1. **Multipass VMs**: Created with defined resources via cloud-init configuration
2. **Networking**: Automatically configured to use the same network interface
3. **RKE2 Installation**: Master node first, then workers join using node token
4. **Kubeconfig**: Generated and configured for remote access
5. **Health Checks**: Automatic verification of node readiness
6. **TLS Configuration**: Master node properly configured with correct TLS SAN entries for secure access

## 🔍 Cluster Health Checks

After deployment, the system automatically verifies that all nodes reach the `Ready` state. The verification process:

- Checks node status every 10 seconds for up to 5 minutes
- Provides a detailed status report of all nodes
- Shows warnings if any nodes fail to reach the `Ready` state
- Displays overall cluster status including system pods

This ensures your cluster is fully functional before you start using it.

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request.
