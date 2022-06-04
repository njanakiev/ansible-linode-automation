# ansible-linode-automation

Ansible playbooks to create and setup [Linode](https://linode.gvw92c.net/LPg90o) cloud instances with a dynamic inventory.

# Installation

This project uses the [linode.cloud](https://github.com/linode/ansible_linode) collection. You need a [Linode](https://linode.gvw92c.net/LPg90o) account with enabled API Token and [Ansible](https://www.ansible.com/) installed to run the following scripts. These scripts were tested with `Ansible 2.11.7` running on `Python 3.7.11`.

Install the required dependencies to run the scripts with:

```bash
ansible-galaxy collection install linode.cloud
pip install linode_api4==5.2.1
pip install polling==0.3.2
```

# Configuration

To configure the servers define the types of servers you want in [config.yml](config.yml) as follows:

```yml
---
server_names:
- server1
- server2

ssh_keys: ~/.ssh/linode.pub

server_type: g6-nanode-1
server_image: linode/ubuntu20.04
server_region: us-east

volume_name: data_volume
volume_size: 100
volume_region: us-east
```

The `ssh_keys` variable denotes the location of the public keys that should be loaded to the server(s). The `server_names` list is only used for [linode-create-multiple-servers.yml](linode-create-servers.yml) and [linode-destroy-servers.yml](linode-destroy-servers.yml). The `volume_*` variables are used in the [linode-create-server-with-volume.yml](linode-create-server-with-volume.yml) script.

# Usage

First, export your `LINODE_API_TOKEN` as a variable from Linode with:

```bash
export LINODE_API_TOKEN=XXXX
```

To create a simple server defined in you can run the playbook:

```bash
ansible-playbook linode-create-server.yml
```

To create multiple servers with docker pre-installed and a dedicated user added, run the playbook:

```bash
ansible-playbook linode-create-multiple-servers.yml \
  --extra-vars "username=user password=XXXXXXX"
```

The server configuration can be found in [config.yml](config.yml).

To list all available servers, type:

```bash
ansible-inventory --list
ansible-inventory --graph
```

To run ad-hoc commands, type:

```bash
ansible all -u root -m ping
ansible all -u root -a "free -m"
```

To delete all servers, run the following playbook:

```bash
ansible-playbook linode-destroy-servers.yml
```

# License 
This project is licensed under the MIT license. See the [LICENSE](LICENSE) for details.
