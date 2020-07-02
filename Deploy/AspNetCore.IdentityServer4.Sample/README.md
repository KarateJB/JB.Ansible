# Deploy karatejb\AspNetCore.IdentityServer4.Sample

The playbook to install Docker and deploy [karatejb\AspNetCore.IdentityServer4.Sample](https://github.com/KarateJB/AspNetCore.IdentityServer4.Sample) to Ubuntu.

## Structure

```s
│  ansible.cfg # Define the inventory and ssh key for the default playbook
│  docker-app-adv.yml  # The playbook for using hosts, group_vars and host_vars inside inventories directory (e.q. inventories/JB)
│  docker-app.yml # The default playbook
│  docker-only.yml # The playbook that only install Docker in managed node
│  inventory # The inventory for default playbook
│
├─inventories
│  └─JB # My custom hosts/group_vars/host_vars/...
│      │  hosts 
│      │
│      ├─group_vars
│      │      all
│      │      development
│      │      production
│      │
│      └─host_vars
│              all
│
└─roles
    ├─app # The role to deploy application (build and start the containers)
    │  ├─defaults
    │  │      main.yml
    │  │
    │  ├─files
    │  │      development.crt
    │  │      development.key
    │  │      development.pfx
    │  │
    │  ├─meta
    │  │      main.yml
    │  │
    │  ├─tasks
    │  │      1.clone_ap_repository.yml
    │  │      2.run_containers.yml
    │  │      3.update_ap_config.yml
    │  │      4.update_cert.yml
    │  │      5.restart_containers.yml
    │  │      6.remove_tmp_files.yml
    │  │      main.yml # Then main task to include the other tasks
    │  │
    │  ├─templates
    │  │  │
    │  │  └─AppSettings
    │  │          AppSettings.Auth.j2 # Template for generating appsettings.Docker.json to Auth Server
    │  │          AppSettings.WebApi.j2 # Template for generating appsettings.Docker.json to Backend web api
    │  │
    │  └─vars
    │          development.yml # Define the variables/values when hosts == "development"
    │          main.yml # Define the default variables/values
    │          production.yml # Define the variables/values when hosts == "production"
    │
    ├─docker # The role to install Docker and Docker-compose
    │  ├─defaults
    │  │      main.yml
    │  │
    │  ├─meta
    │  │      main.yml
    │  │
    │  └─tasks
    │          main.yml
    │
    └─pip  # The role to install python3's related packages
        └─tasks
                main.yml
```


## Steps (Using default playbook)


1. Update the managed node's information in `inventory`

For example, 

```
[dev_hp_omen]
127.0.0.1 ansible_port=22 ansible_user=jb ansible_python_interpreter=/usr/bin/python3
```


2. (Optional) Update the configurations of Auth Server or Backend webapi

If you wanna change the values of AppSettings, update the values of

`roles/app/vars/development.yml` (For development environment)

or create a new vars file.


3. Run the playbook

```s
$ ansible-playbook docker-app.yml -e "machine=dev_hp_omen env=development"
```

The above command will load inventory/ssh key that are defined in `ansible.cfg`.
You can bypass the **extra vars**: `machine`, `env`, by overwriting the values in `docker-app.yml`:

```yml
 vars:
    - machine: dev_hp_omen
    - env: "development"
```



## Steps (Using hosts/group_vars/host_vars in invertories/xxx)


1. Update the managed node's information 

Define the managed node's IP in `inventories/xxx/group_vars/all` and

Update the managed node's group information in `inventories/xxx/hosts`.


2. (Optional) Update the configurations of Auth Server or Backend webapi

If you wanna change the values of AppSettings, update the values of

`inventories/xxx/group_vars/development` (For development environment)

or create a new vars file.


3. Run the playbook

```s
$ ansible-playbook -e "env=development" --private-key ~/.ssh/id_rsa -i ./inventories/xxx/ docker-app-adv.yml
```

The **extra vars**: `env`, can be bypass by by overwriting the values in `docker-app-adv.yml`:

```yml
vars:
    - env: development
```

E.q.

```s
$ ansible-playbook -e "env=development" --private-key ~/.ssh/id_rsa -i ./inventories/JB/ docker-app-adv.yml
```



## Trouble shootings

### Debug

Debug Ansible by 

```s
ANSIBLE_DEBUG=True ansible-inventory -i inventories/JB --list --yaml
```


### WSL2

#### Docker 'cgroups: cannot find cgroup mount destination: unknown'

See https://github.com/microsoft/WSL/issues/4189#issuecomment-518277265

```s
$ sudo mkdir /sys/fs/cgroup/systemd
$ sudo mount -t cgroup -o none,name=systemd cgroup /sys/fs/cgroup/systemd
```