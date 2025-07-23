## VPS Bootstrap

This role deletes ssh root access, and creates an admin user wich can only 
authenticate via ssh key. 
If the configured ssh key already exists on the host machine, it will be copied.
Otherwise the ssh key pair will be generated first.

example configuration:

```
vps_bootstrap__user:
  name: "admin"
  key_comment: "vps admin"
  # If this key does not exist it will be generated
  key_path: "~/.ssh/id_emanuel_staging"

```
