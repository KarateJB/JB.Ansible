# JB.Ansible

Ansible playbooks for demo, deploy ...etc


## Basic commands

### Run the Ansible Playbook

```s
$ ansible-playbook --private-key ~/.ssh/id_rsa -i inventory playbook.yml
```

Or use the configuration from `ansible.cfg`, update the current work directory to o-w 

```s
$ chmod o-w .
$ ansible-playbook playbook.yml
```


## Deploy/AspNetCore.IdentityServer4.Sample

```s
$ ansible-playbook docker-app.yml -e "machine=dev_hp_omen env=development"
```


> The `env` variable's defualt value is defined in `docker-app.yml`.

> Debug Ansible by `ANSIBLE_DEBUG=True ansible-inventory -i inventories/dev --list --yaml` for example.



For advanced inventory/vars usage (group_vars/hosts_vars/hosts), use the following command,

```s
$ ansible-playbook -e "env=development" --private-key ~/.ssh/id_rsa -i ./inventories/JB/ docker-app-adv.yml
```


## Trouble shootings

### WSL2

#### Docker 'cgroups: cannot find cgroup mount destination: unknown'

See https://github.com/microsoft/WSL/issues/4189#issuecomment-518277265

```s
$ sudo mkdir /sys/fs/cgroup/systemd
$ sudo mount -t cgroup -o none,name=systemd cgroup /sys/fs/cgroup/systemd
```