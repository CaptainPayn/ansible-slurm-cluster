## ansible slurm cluster
- you must generate munge key before running it
```bash
dd if=/dev/urandom bs=1 count=1024 of=roles/munge/files/munge.key
```
