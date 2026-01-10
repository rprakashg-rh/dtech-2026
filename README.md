# dtech-2026
Repository contains all artifacts used to build dtech-2026 demo setup

## Pre-flight steps
Obtain an Open AWS account in [demo.redhat.com](https://demo.redhat.com) and provision an EC2 host which will be used for building customized RHEL system image to be used for provisioning the Advantech ECU579 host.

Install [this](https://github.com/rprakashg/vpac) ansible collection by running command below

Create an ansible inventory file and add snippet below, replace with specific parameters 

```yaml
---
all:
  hosts:
    imagebuilder:
      ansible_host: "<replace>"
      ansible_port: 22
      ansible_user: "<replace>"
      ansible_ssh_private_key_file: "<replace>"
```

Install ansible vpac collection by running command below

```sh
ansible-galaxy collection install git+https://github.com/rprakashg/vpac.git,main
```

## Build Custom ISO

### Provision Image Builder Server

### Setup Image Builder
Steps to configure host as image builder server is automated in [playbook](../playbooks/setup_imagebuilder.yml). Before we can go ahead and run this playbook as shown in command below


```sh
ansible-playbook -i inventory setup-imagebuilder.yml 
```

### Build ISO

Set vault secret in environment variable

```sh
export VAULT_SECRET=<redacted>
```

```sh
ansible-playbook -i inventory --vault-password-file <(echo "$VAULT_SECRET") build_iso.yml -e @vars/vpac-system.yml
```
