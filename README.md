
# K3s Home AI stack playbook

## Thanks!
Many thanks to [itwars](https://github.com/itwars) for the [K3s playbook](https://github.com/itwars/k3s-ansible), from which this repo was forked

## What does it do
The playbook has been only tested on fresh minimal installs of Ubuntu 20.04~24.04 with amd64 and arm64 architecture.
The playbook assumes you have an NFS, ELK servers and local docker registry.
It will attempt to provision the K3s nodes, NVidia drivers, self-signed cert issuer and the apps listed below.
Exclude what you don't need in [site.yml](https://github.com/branep/k3s-ai-stack-ansible/blob/main/site.yml)

```yaml
roles:
    - role: apps/node-feature-discovery
    - role: apps/whisper-asr
    - role: apps/ollama
    - role: apps/mimic3
    - role: apps/fasttext
    - role: apps/open-webui
    - role: apps/rancher
    - role: apps/filebeat
```

I have not found Mimic3 images available in public registries, hence the local registry addition.


## Usage
Remove unneeded roles from [the playbook](https://github.com/branep/k3s-ai-stack-ansible/blob/main/site.yml)

Edit [all.yml](https://github.com/branep/k3s-ai-stack-ansible/blob/main/inventory/example/group_vars/all.yml) and replace with your local network values.

Update the [inventory](https://github.com/branep/k3s-ai-stack-ansible/blob/main/inventory/example/hosts.ini) with your nodes.

Setup the virtual environment in the path of the repo:
```bash
python -m venv ./
source ./bin/activate
pip install -r requirements.txt
```

Make sure to use ansible-vault for your secret values.

Run the playbook example:
```bash
./bin/ansible-playbook site.yml -i inventory/example/hosts.ini \
    --ask-pass --ask-become-pass  --ask-vault-pass \
    -e "reboot=true do_bootstrap=true"
```
