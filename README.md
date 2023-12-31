# shovel_ansible

 Install [shovel](https://github.com/bbkane/shovel) + [OpenObserve](https://openobserve.ai/) as systemd services, on a local dev VM or production VM.

# Local Dev VM install

## Start a local Lima VM

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

## Install shovel + OpenObserve on the Lima VM

Obtain a shovel release from [GitHub](https://github.com/bbkane/shovel/releases) or by building with GoReleasor

Get an [OpenObserve release](https://github.com/openobserve/openobserve/releases).

Update [./lima_vars.yaml](./lima_vars.yaml) with paths to the releases

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

## Open sites

Open OpenObserve at: http://localhost:5080/web/traces?period=15m&query=&org_identifier=default

Open shovel at: http://127.0.0.1:8080/?count=1&nameservers=dns3.p09.nsone.net%3A53&protocol=udp&qnames=linkedin.com+www.linkedin.com&rtypes=A&subnetMap=&subnets=

## Import dashboards

There isn't an API for this yet, so export by using the download icon when viewing a dashboard and import from the main menu.

# Production VM

Obtain releases and export envvars as above.

Create `prod_vars.yaml`. Use `./lima_vars.yaml` for reference

```bash
ansible-playbook -i 'host,' --extra-vars "@prod_vars.yaml" openobserve.ansible.yaml shovel.ansible.yaml
```

# Debugging

```bash
# Check if the service is running
sudo systemctl status shovel

# Check logs
sudo journalctl -u shovel
```
