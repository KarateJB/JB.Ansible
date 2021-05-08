# JB.Ansible

Ansible playbooks for demo, deploy ...etc.




## Content

| Type | Path | Description |
|:----:|:-----|:------------|
| Demo | Demo/01.Get_Started | A sample playbook to get started |
| Deploy | Deploy/AspNetCore.IdentityServer4.Sample | The playbook to install Docker and deploy [karatejb\AspNetCore.IdentityServer4.Sample](https://github.com/KarateJB/AspNetCore.IdentityServer4.Sample) to Ubuntu |





## Basic commands

### Run the Ansible Playbook

```s
$ ansible-playbook --private-key ~/.ssh/id_rsa -i inventory playbook.yml
```

Or use the configuration from `ansible.cfg`, notice that Ansible may have a warning like this


> Ansible is being run in a world writable directory (/dev/ansible/Init/Minikube), ignoring it as an ansible.cfg source.


We have to update the current work directory to o-w 

```s
$ chmod o-w .
$ ansible-playbook playbook.yml
```

If you dont have the permission, just do

```bash
$ export ANSIBLE_CONFIG=<path of ansible.cfg>
```






## Trouble shootings

### WSL2

#### Docker 'cgroups: cannot find cgroup mount destination: unknown'

See https://github.com/microsoft/WSL/issues/4189#issuecomment-518277265

```s
$ sudo mkdir /sys/fs/cgroup/systemd
$ sudo mount -t cgroup -o none,name=systemd cgroup /sys/fs/cgroup/systemd
```