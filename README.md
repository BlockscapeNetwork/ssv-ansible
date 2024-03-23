# SSV Ansible Role

This is an Ansible role for installing and configuring the SSV`s DVT solution.

## Description

This role installs and configures the SSV`s DVT solution on a target machine. The role is designed to be used in an existing Ethereum validator setup.

## Getting Started

### Dependencies

docker, pip

```bash
- role: geerlingguy.docker
- role: geerlingguy.pip
    pip_install_packages:
    - name: docker
```

### Installing

Example Playbook with use of [EthPandaOps Ansible Collection](https://github.com/ethpandaops/ansible-collection-general/tree/master/roles/ethereum_node):

```bash
- hosts: <Your-Linux-Host>
  become: true
  roles:
  - role: geerlingguy.docker
  - role: geerlingguy.pip
    pip_install_packages:
    - name: docker
  - role: ethpandaops.general.ethereum_node
    ethereum_node_el: geth
    ethereum_node_cl: lighthouse
    ethereum_node_mev_boost_enabled: true
    ethereum_node_cl_validator_enabled: true
    ethereum_node_cl_validator_fee_recipient: "<your_fee_recipient>"
  - role: blockscape.ssv
```

### Configuration

Follow the steps from SSV documentation to generate the encrypted key: https://docs.ssv.network/operator-user-guides/operator-node/installation#generate-operator-keys-encrypted

Create the configfile according to the SSV documentation and point it to your EL &  CL clients: https://docs.ssv.network/operator-user-guides/operator-node/installation#create-configuration-file

copy the encrypted key & config to the target machine and set the path in the ansible playbook (default path: /data/ssv).

EXAMPLE (both files are in the a folder called "ssv" beside your playbook):

```bash
- name: Copy ssv configs to remote server
  copy:
    src: "{{ playbook_dir }}/ssv/"
    dest: "/data/ssv"
```

## License

This project is licensed under the [MIT License](LICENSE) - see the LICENSE.md file for details.
