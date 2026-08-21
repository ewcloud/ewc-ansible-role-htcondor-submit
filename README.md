# HTCondor Submit Node Ansible Role

This repository contains a configuration template 
(i.e. an [Ansible Role](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_reuse_roles.html)) 
to customize your environment in the
[European Weather Cloud (EWC)](https://europeanweather.cloud/).
The template is designed to:
* Configure a pre-existing virtual machine, running RockyLinux version 8 or 9 and with a minimum recommended 8 CPU cores and 32GB of RAM, such that it:
    * Enables users to submit their compute jobs to a shared pool of HTCondor Execute nodes running within the EWC.

## Copyright and License
Copyright © EUMETSAT 2026.

The provided code and instructions are licensed under the [MIT license](./LICENSE).
They are intended to automate the setup of an environment that includes 
third-party software components.
The usage and distribution terms of the resulting environment are 
subject to the individual licenses of those third-party libraries.

Users are responsible for reviewing and complying with the licenses of
all third-party components included in the environment.

Contact [EUMETSAT](http://www.eumetsat.int) for details on the usage and distribution terms.

## Usage

The step-by-step described below assume your local file system follows the 
example structure below, with `ewc-ansible-role-htcondor-submit` being a clone of this
repository:
```
.
├── roles
│   └── ewc-ansible-role-htcondor-submit
├── inventory.yml
└── playbook.yml
```

### 1. Specify the target host and SSH credentials
Create an inventory file to specify address/credentials that Ansible should use
to reach the virtual machine you wish to configure:
```yaml
# inventory.yml
---
ewcloud:
  hosts:
    htc_submit:
      ansible_python_interpreter: auto
      ansible_host: <add the IP address of the target host>
      ansible_ssh_private_key_file: <add the path to local SSH private key file>
      ansible_user: cloud-user
```
### 2. Customize the template

Edit input values for the template [variables](vars/main.yml) as needed (see [Inputs](#inputs) section for details).
Then, proceed to create an Ansible Playbook file to load your customizations:

```yaml
# playbook.yml
---
- name: Install HTCondor Submit
  hosts: htcondor_submit
  become: true
  become_user: root
  become_method: ansible.builtin.sudo

  roles:
    - ewc-ansible-role-htcondor-submit
```

### 3. Apply the template

You can apply changes on the target host by running:
```bash
ansible-playbook -i inventory.yml playbook.yml
```
## Inputs

| Name                        | Description                                                                                        | Type     | Default  | Required |
| --------------------------- | ----------------------------------------------------------------------------------------------     | -------- | -------- | :------: |
| headscale_login_server      | URI of the VPN server.  Example: `https://headscale.example.org`                                   | `string` | n/a      |    yes   |
| headscale_preauthkey        | Credentials of the VPN server.                                                                     | `string` | n/a      |    yes   |
| htcondor_cm_external_ip     | IP Address of the HTCondor Manager node to which this submit node reports to. Example: `10.0.0.15` | `string` | n/a      |    yes   |
| htcondor_password           | Password to authenticate against HTCondor Execute node pool                                        | `string` | n/a      |    yes   |

## Dependencies
> 💡 Upon execution, a SBOM (SPDX format) is auto-generated and stored in the VM's file system root directory (see `/sbom.json`).
Third-party components used in the resulting environment.

| Component |  Home URL |
|------|---------|
| tailscale | https://github.com/tailscale/tailscale |
| htcondor | https://github.com/htcondor/htcondor |

## Changelog
All notable changes (i.e. fixes, features and breaking changes) are documented 
in the [CHANGELOG.md](./CHANGELOG.md).

## Contributing

Thanks for taking the time to join our community and start contributing!
Please make sure to:
* Familiarize yourself with our [Code of Conduct](./CODE_OF_CONDUCT.md) before 
contributing.
* See [CONTRIBUTING.md](./CONTRIBUTING.md) for instructions on how to request 
or submit changes.

## Authors

[European Weather Cloud](http://support.europeanweather.cloud/) 
<[support@europeanweather.cloud](mailto:support@europeanweather.cloud)>