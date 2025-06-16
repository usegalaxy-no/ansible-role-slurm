Role Name
=========

This role installs and configures Slurm on both controller and exec nodes.
Nodes are automatically registered to the Slurm cluster. To delete a node from the cluster, you must manually run `scontrol delete nodename=<nodename>`.

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
| `slurm_create_home`           | `no`                                    | Whether to create a home directory for Slurm  |
| `slurm_conf_template`         | `templates/slurm.conf.j2`               | Path to the Slurm configuration template      |
| `slurm_conf_dest`             | `/etc/slurm/slurm.conf`                 | Destination for the Slurm configuration file  |
| `slurm_conf_owner`            | `slurm`                                 | Owner of the Slurm configuration file         |
| `slurm_conf_group`            | `slurm`                                 | Group of the Slurm configuration file         |
| `slurm_conf_mode`             | `0644`                                  | Permissions for the Slurm configuration file  |
| `slurm_munge_key_src`         | `files/slurm/munge.key`                 | Source path for the Munge key                 |
| `slurm_munge_key_dest`        | `/etc/munge/munge.key`                  | Destination path for the Munge key            |
| `slurm_munge_key_owner`       | `munge`                                 | Owner of the Munge key                        |
| `slurm_munge_key_group`       | `munge`                                 | Group of the Munge key                        |
| `slurm_munge_key_mode`        | `0400`                                  | Permissions for the Munge key                 |
| `slurm_directories`           | `['/var/spool/slurmd', '/var/log/slurm']` | Directories to create for Slurm services    |
| `slurm_cluster_name`          | `dev-cluster`                           | Name of the Slurm cluster                     |
| `slurm_control_machine`       | `localhost`                             | Hostname or IP of the Slurm control machine   |
| `slurm_slurmd_user`           | `root`                                  | User for Slurmd services                      |
| `slurm_state_save_location`   | `/var/spool/slurmctld`                  | Directory for saving Slurm state              |
| `slurm_slurmd_spool_dir`      | `/var/spool/slurmd`                     | Directory for Slurmd spool                    |
| `slurm_ctld_log_file`         | `/var/log/slurm/slurmctld.log`          | Log file for Slurm controller                 |
| `slurm_d_log_file`            | `/var/log/slurm/slurmd.log`             | Log file for Slurmd                           |
| `slurm_ctld_pid_file`         | `/run/slurmctld.pid`                    | PID file for Slurm controller                 |
| `slurm_d_pid_file`            | `/run/slurmd.pid`                       | PID file for Slurmd                           |
| `slurm_ctld_timeout`          | `120`                                   | Timeout for Slurm controller                  |
| `slurm_d_timeout`             | `120`                                   | Timeout for Slurmd                            |
| `slurm_partition_name`        | `dev-cluster`                           | Name of the Slurm partition                   |
| `slurm_partition_max_time`    | `2-00:00:00`                            | Maximum runtime for jobs in the partition     |
| `slurm_cgroup_conf_template`  | `templates/cgroup.conf.j2`              | Path to the cgroup configuration template     |
| `slurm_cgroup_conf_dest`      | `/etc/slurm/cgroup.conf`                | Destination for the cgroup configuration file |
| `slurm_cgroup_conf_owner`     | `slurm`                                 | Owner of the cgroup configuration file        |
| `slurm_cgroup_conf_group`     | `slurm`                                 | Group of the cgroup configuration file        |
| `slurm_cgroup_conf_mode`      | `0644`                                  | Permissions for the cgroup configuration file |
| `slurm_max_node_count`        | `5`                                     | Maximum number of nodes in the Slurm cluster  |
| `slurm_playbook_type`         | `exec`                                  | Determines which playbook to run (`controller` or `exec`) |
| `slurm_accounting_storage_host` | `localhost`                           | Hostname for Slurm accounting storage         |
| `slurm_accounting_storage_type` | `accounting_storage/slurmdbd`           | Type of Slurm accounting storage              |
| `slurm_crypto_type`           | `crypto/munge`                          | Type of cryptography used by Slurm            |
| `slurm_def_mem_per_cpu`       | `1000`                                  | Default memory per CPU (in MB)                |
| `slurm_job_acct_gather_type`  | `jobacct_gather/cgroup`                 | Type of job accounting data gathering         |
| `slurm_proctrack_type`        | `proctrack/cgroup`                      | Type of process tracking                      |
| `slurm_select_type_parameters`| `CR_Core_Memory`                        | Parameters for SelectType                     |
| `slurm_slurmctld_port`        | `6817`                                  | Port for Slurm controller daemon              |
| `slurm_slurmd_debug`          | `3`                                     | Debug level for Slurmd                        |
| `slurm_slurmd_port`           | `6818`                                  | Port for Slurmd daemon                        |
| `slurm_slurmdbd_conf_template`| `templates/slurmdbd.conf.j2`            | Path to the Slurmdbd configuration template   |
| `slurm_slurmdbd_conf_dest`    | `/etc/slurm/slurmdbd.conf`              | Destination for the Slurmdbd configuration file|
| `slurm_slurmdbd_conf_owner`   | `slurm`                                 | Owner of the Slurmdbd configuration file      |
| `slurm_slurmdbd_conf_group`   | `slurm`                                 | Group of the Slurmdbd configuration file      |
| `slurm_slurmdbd_conf_mode`    | `0600`                                  | Permissions for the Slurmdbd configuration file|
| `slurm_slurmdbd_log_file`     | `/var/log/slurm/slurmdbd.log`           | Log file for Slurmdbd                         |
| `slurm_slurmdbd_pid_file`     | `/run/slurmdbd.pid`                     | PID file for Slurmdbd                         |
| `slurm_slurmdbd_storage_type` | `mysql`                                 | Type of database for Slurmdbd storage         |
| `slurm_slurmdbd_storage_host` | `localhost`                             | Hostname for Slurmdbd database                |
| `slurm_slurmdbd_storage_port` | `3306`                                  | Port for Slurmdbd database                    |
| `slurm_slurmdbd_storage_user` | `slurm`                                 | User for Slurmdbd database                    |
| `slurm_slurmdbd_storage_pass` | `password`                              | Password for Slurmdbd database (**CHANGE THIS**)|
| `slurm_slurmdbd_storage_db`   | `slurm_acct_db`                         | Database name for Slurmdbd storage            |
| `nfs_server`                  | `true`                                  | Set to true to enable NFS server installation |
| `nfs_export_path`             | `/opt/pulsar/files/staging/`            | The directory to export via NFS               |
| `nfs_allowed_clients`         | `*(rw,sync,no_subtree_check)`           | Clients and options for NFS export            |
| `nfs_client`                  | `true`                                  | Set to true to enable NFS client mounting     |
| `nfs_server_ip`               | `your_nfs_server_ip`                    | IP or hostname of the NFS server              |
| `nfs_remote_path`             | `/export/slurm_data`                    | The path on the NFS server to mount           |
| `nfs_mount_point`             | `/mnt/slurm_data`                       | The local directory to mount the NFS share    |

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

To run this role, set the `slurm_playbook_type` variable to either `controller` or `exec` depending on the type of node you are configuring.

### Example Command

```bash
ansible-playbook -i inventory_file playbook.yml -e slurm_playbook_type=controller
```

Example Playbook
----------------

```yaml
- hosts: all
  become: true
  vars:
    slurm_munge_key_src: files/slurm/munge.key
    slurm_playbook_type: exec
  roles:
    - slurm
```
