# JB.Ansible

Ansible playbooks for demo, deploy ...etc


## Run the Ansible Playbook

```s
$ ansible-playbook --private-key ~/.ssh/id_rsa -i inventory playbook.yml
```

Or use the configuration from `ansible.cfg`, update the current work directory to o-w 

```
$ chmod o-w .
$ ansible-playbook playbook.yml
```



## Trouble shootings

### WSL2

https://github.com/microsoft/WSL/issues/4189#issuecomment-518277265

```s
$ sudo mkdir /sys/fs/cgroup/systemd
$ sudo mount -t cgroup -o none,name=systemd cgroup /sys/fs/cgroup/systemd
```