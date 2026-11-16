## ansible slurm cluster
* ansible.cfg points to inventory/hosts.ini and roles/ and enables privilege escalation
* inventory/hosts.ini defines cluster across 3 groups all under slurm_cluster:children
* inventory/group_vars/all.yml source of truth (UIDs, GIDs, paths, ports, nfs subnet)
* site.yml master playbook
## common --> target group: slurm_cluster
* installs baseline utilities and configures time sync (chrony)
## users --> target group: slurm_cluster
* creates slurmuser account with identical uid and gid across rhel or deb so nfs file ownership matches everywhere
## nfs --> target group: slurm_cluster
* configures /etc/nfs-server and starts nfs-server then mounts via fstab on all other nodes
## firewall --> target group: slurm_cluster
* branches by ansible_os_family; firewalld and ufw, ssh, slurm daemons, srun ports, and nfs traffic
## munge --> target_group: slurm_cluster
## slurm_common --> target group: slurm_cluster
## slurm_controller --> target group: slurm_head
## slurm_worker --> target group: slurm_workers

* you must generate munge key before running it
```bash
dd if=/dev/urandom bs=1 count=1024 of=roles/munge/files/munge.key
```
* then make sure hosts.ini has the updated hostnames and run
```bash
ansible-playbook site.yml
```
* you can optionally pass in `--limit hostname` to just run it against one node

* updated slurm install from nfs
* added cgroup deploy