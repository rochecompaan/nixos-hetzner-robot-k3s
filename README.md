# My NixOS K3S Cluster

My NixOS based K3S Cluster configuration hosted on my personal Git Server. Feel free to look around. Be aware that not all configuration files are available in my public repository.

**NOTE:** This repository is currently a proof of concept for my upcoming K3s Cluster, it has not yet been used productively!

## Overview

This repository provides the **Infrastructure as Code**[^1] (IaC) and **GitOps**[^2] State for the following tools:

- [**NixOS**](https://nixos.org/): Linux distribution based on Nix to provide a declarative and reproducible system.
- [**K3S**](https://k3s.io/): Lightweight certified Kubernetes distribution.
- [**Flux**](https://github.com/fluxcd/flux2): GitOps Kubernetes Operator that ensures that my cluster state matches the desired state described in this repository.
- [**Renovate**](https://github.com/renovatebot/renovate): Automatically updates third-party dependencies declared in my Git repository via pull requests.
- [**SOPS**](https://github.com/mozilla/sops): Tool for managing secrets.

[^1]: Infrastructure as Code (IaC) is the process of managing and provisioning computer infrastructure through configuration files.
[^2]: GitOps is an operational framework that takes best practices from application development such as version control, collaboration, compliance, and CI/CD, and applies them to infrastructure automation.

## Description

A single node k3s cluster which can be fully reproducibly deployed with a single command. At the same time the node contains the git server which represents the GitOps state of the cluster. To solve the chicken edge problem an additional git server is configured on operating system level which has partial access to the repositories of the main git server which is running in k3s to have the cluster configurable even in case of configuration errors.

## Setup

### Install

Boot the nixos minimal iso and set the root user password to access the live iso via ssh. Then run the following command on remote computer:

```bash
nix run '.#install-system' -- supermicro-k3s root@${IP}
```

### Update (System)

This is only necessarry when the NixOS system configuration in this repository has changed. All configuration insied the `./kubernetes` directory will automaticaly applied by Flux and don't need manuall interaction with the cluster.

```bash
nix run '.#update-system' -- supermicro-k3s ${IP}
```

<br>
