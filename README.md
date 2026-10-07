## ansible slurm cluster
- you must generate munge key before running it
```bash
dd if=/dev/urandom bs=1 count=1024 of=roles/munge/files/munge.key
```
- then make sure hosts.ini has the updated hostnames and run
```bash
ansible-playbook site.yml
```
- you can optionally pass in --limit `hostname` to just run it against one node

## updated slurm install from nfs
## added cgroup deploy
