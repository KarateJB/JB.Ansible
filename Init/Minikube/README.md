# Deploy karatejb\AspNetCore.IdentityServer4.Sample

The playbook to install Docker and deploy [karatejb\AspNetCore.IdentityServer4.Sample](https://github.com/KarateJB/AspNetCore.IdentityServer4.Sample) to Ubuntu.


***
## Structure



***
## Steps (Using default playbook)




### 1. Update the managed node's information in `inventory`

For example, 

```
[dev_hp_omen]
127.0.0.1 ansible_port=2244 ansible_user=jb ansible_python_interpreter=/usr/bin/python3
```

### 2. Run the playbook

```s
$ ansible-playbook minikub.yml -e "machine=dev_hp_omen"
```

The above command will load inventory/ssh key that are defined in `ansible.cfg`.
You can bypass the **extra vars**: `machine`, `env`, by overwriting the values in `docker-app.yml`:

```yml
 vars:
    - machine: dev_hp_omen
    - env: "development"
```


***
## Trouble shootings

### Debug

Debug Ansible by 

```s
ANSIBLE_DEBUG=True ansible-inventory -i inventories/JB --list --yaml
```


### Cannot have both the docker-py and docker python modules

1. Uninstall python2

```s
$ apt-get purge -y python2.7
```


2. Uninstall docker's related package

```s
$ pip3 list | grep docker
docker (4.3.1)
docker-compose (1.26.2)
docker-pycreds (0.4.0)
dockerpty (0.4.1)

$ pip3 uninstall docker docker-compose
```


3. Then force re-install docker and docker-compose.

```s
$ pip3 install --force-reinstall docker docker-compose
```

And restart the machine.



### WSL2

#### Docker 'cgroups: cannot find cgroup mount destination: unknown'

See https://github.com/microsoft/WSL/issues/4189#issuecomment-518277265

```s
$ sudo mkdir /sys/fs/cgroup/systemd
$ sudo mount -t cgroup -o none,name=systemd cgroup /sys/fs/cgroup/systemd
```