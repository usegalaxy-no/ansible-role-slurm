Role Name
=========

This role installs and configures Slurm on both controller and exec nodes.
All nodes should have hostnames as defined in `slurm_node_name` and must also be reachable via DNS to allow dynamic Slurm configuration.
The range [001-010] in the node and partition name defines how many nodes you have in your Slurm cluster.

Requirements
------------

- Ansible 2.9 or later
- Supported OS: AlmaLinux, CentOS, or similar

Role Variables
--------------

| Variable                      | Default Value                           | Description                                   |
|-------------------------------|-----------------------------------------|-----------------------------------------------|
| `slurm_user`                  | `slurm`                                 | User for Slurm services                       |
| `slurm_group`                 | `slurm`                                 | Group for Slurm services                      |
| `slurm_uid`                   | `640`                                   | UID for the Slurm user                        |
| `slurm_gid`                   | `640`                                   | GID for the Slurm group                       |
| `slurm_shell`                 | `/bin/bash`                             | Shell for the Slurm user                      |
| `slurm_create_home`           | `no`                                    | Whether to create a home directory for Slurm   |
| `slurm_conf_template`         | `templates/slurm/slurm.conf.j2`         | Path to the Slurm configuration template       |
| `slurm_conf_dest`             | `/etc/slurm/slurm.conf`                 | Destination for the Slurm configuration file   |
| `slurm_conf_owner`            | `slurm`                                 | Owner of the Slurm configuration file          |
| `slurm_conf_group`            | `slurm`                                 | Group of the Slurm configuration file          |
| `slurm_conf_mode`             | `0644`                                  | Permissions for the Slurm configuration file   |
| `slurm_munge_key_src`         | `files/slurm/{{ run_env }}/munge.key`   | Source path for the Munge key                  |
| `slurm_munge_key_dest`        | `/etc/munge/munge.key`                  | Destination path for the Munge key             |
| `slurm_munge_key_owner`       | `munge`                                 | Owner of the Munge key                         |
| `slurm_munge_key_group`       | `munge`                                 | Group of the Munge key                         |
| `slurm_munge_key_mode`        | `0400`                                  | Permissions for the Munge key                  |
| `slurm_directories`           | `['/var/spool/slurmd', '/var/log/slurm']` | Directories to create for Slurm services    |
| `slurm_cluster_name`          | `dev-cluster`                           | Name of the Slurm cluster                      |
| `slurm_control_machine`       | `localhost`                             | Hostname or IP of the Slurm control machine    |
| `slurm_slurmd_user`           | `root`                                  | User for Slurmd services                       |
| `slurm_state_save_location`   | `/var/spool/slurmctld`                  | Directory for saving Slurm state               |
| `slurm_slurmd_spool_dir`      | `/var/spool/slurmd`                     | Directory for Slurmd spool                     |
| `slurm_ctld_log_file`         | `/var/log/slurm/slurmctld.log`          | Log file for Slurm controller                  |
| `slurm_d_log_file`            | `/var/log/slurm/slurmd.log`             | Log file for Slurmd                            |
| `slurm_ctld_pid_file`         | `/run/slurmctld.pid`                    | PID file for Slurm controller                  |
| `slurm_d_pid_file`            | `/run/slurmd.pid`                       | PID file for Slurmd                            |
| `slurm_ctld_timeout`          | `120`                                   | Timeout for Slurm controller                   |
| `slurm_d_timeout`             | `120`                                   | Timeout for Slurmd                             |
| `slurm_node_name`             | `compute[001-010]`                      | Node name pattern                              |
| `slurm_sockets`               | `1`                                     | Number of sockets per node                     |
| `slurm_cores_per_socket`      | `8`                                     | Number of cores per socket                     |
| `slurm_threads_per_core`      | `2`                                     | Number of threads per core                     |
| `slurm_real_memory`           | `64000`                                 | Real memory available per node (in MB)         |
| `slurm_partition_name`        | `dev-cluster`                           | Name of the Slurm partition                    |
| `slurm_partition_nodes`       | `compute[001-010]`                      | Nodes in the partition                         |
| `slurm_partition_max_time`    | `2-00:00:00`                            | Maximum runtime for jobs in the partition      |
| `slurm_cgroup_conf_template`  | `templates/cgroup.conf.j2`        | Path to the cgroup configuration template     |
| `slurm_cgroup_conf_dest`      | `/etc/slurm/cgroup.conf`                | Destination for the cgroup configuration file |
| `slurm_cgroup_conf_owner`     | `slurm`                                 | Owner of the cgroup configuration file        |
| `slurm_cgroup_conf_group`     | `slurm`                                 | Group of the cgroup configuration file        |
| `slurm_cgroup_conf_mode`      | `0644`                                  | Permissions for the cgroup configuration file |

Dependencies
------------

None.

How to Generate the Munge Key
-----------------------------

To generate the Munge key, run the following command on a secure system:

```bash
sudo /usr/sbin/create-munge-key
```

The generated key will be located at `/etc/munge/munge.key`. Copy this key to the appropriate location specified by the `slurm_munge_key_src` variable in your playbook.

How to Install the Role
-----------------------

To install this role using a requirements file, add the following entry to your `requirements.yml`:

```yaml
- src: https://github.com/usegalaxy-no/ansible-role-slurm.git
  scm: git
  version: main
  name: slurm
```

Then, run the following command to install the role:

```bash
ansible-galaxy install -r requirements.yml
```

How to Run
----------

To run this role, ensure the inventory file specifies the groups `slurm_controller` and/or `slurm_exec`.

### Example Command

```bash
ansible-playbook -i inventory_file playbook.yml
```

### Group-Specific Instructions

- **Slurm Controllers**: Ensure the host is in the `slurm_controller` group in your inventory file. This will execute tasks defined in `slurm-controller-install.yaml`.
- **Slurm Exec Nodes**: Ensure the host is in the `slurm_exec` group in your inventory file. This will execute tasks defined in `slurm-exec-install.yaml`.

Make sure the inventory file and variables are properly configured before running the playbook.

Example Playbook
----------------

```yaml
- hosts: slurm_controller
  become: true
  vars:
    slurm_munge_key_src: files/slurm/munge.key
  roles:
    - slurm
```
