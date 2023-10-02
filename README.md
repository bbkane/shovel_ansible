# shovel_ansible

 Install shovel + OpenObserve as systemd services, on a local dev VM or production VM.

# Local VM (Lima)

Mostly from [Lima: a nice way to run Linux VMs on Mac](https://jvns.ca/blog/2023/07/10/lima--a-nice-way-to-run-linux-vms-on-mac/)

Install Lima:

```bash
brew install lima
```

Useful Lima commands:

```bash
limactl start default
lima  # open shell
limactl factory-reset default  # to test playbook from scratch
limactl stop
```

### Install shovel on Lima VM

```bash
export SHOVEL_SERVE_OPENOBSERVE_PASS='...';
export SHOVEL_SERVE_OPENOBSERVE_USER='...';
export ZO_ROOT_USER_EMAIL='...';
export ZO_ROOT_USER_PASSWORD='...';
```

```bash
limactl show-ssh default -f config > ./ssh.cfg
```

```bash
ansible-playbook -i 'lima-default,' --ssh-extra-args '-F ssh.cfg' --extra-vars "@lima_vars.yaml" openobserve.ansible.yaml shovel.ansible.yaml
```

# Production VM

Export envvars as above.

Create `prod_vars.yaml`

```bash
ansible-playbook -i 'host,' --extra-vars "@prod_vars.yaml" openobserve.ansible.yaml shovel.ansible.yaml
```

