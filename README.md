# Ansible environment setup

## Ansible is a python product, so let's operate with it as any python programm.

### Requirements
- Any os with bash, zsh or other *nix-style command promt
- python 3.10.x
- virtualenv using wanted!
- ansible-core above 2.13.0 but lower 2.13.6

> If you are installing software not to virtualenv or --user 
> it's your own risk to broke security or working environment 
> using highest privileges

## Ansible dependencies configuration

Make sure you have uninstalled all other versions to avoid broken dependencies
(better option is uuse virtualenv!)

### Prepare local env to running ansible
#### Setting env
```shell
pip install --upgrade pip virtualenv
python -m virtualenv $HOME/ansible-core2.13
source $HOME/ansible-core2.13/bin/activate
pip install --upgrade -r requirements.txt
ansible-galaxy install -r requirements.yml
```

## Molecule preparation
  * Complete meta/main.yml from your role with template from __templates_role/meta.yml__
  * Add requirements.yml to root of your role with all depenencies using as template file
  __templates_role/requirements.yml__ if you have any dependencies for your role


#### Make sure you have same linter configs in root of you role as all other
 - copy from templates_linter to your role
 - check your role with commands:
```shell
yamllint ./ ;\
ansible-lint ./ -v
```
fix all errors before continue

Run in root folder of your role
```shell
molecule init scenario --role-name YOUR_ROLE_NAME --driver-name ec2


#### Running molecule

Test your molecule config by
```shell
 molecule create
```

Run tests by
```shell
molecule test --destroy=never -- -b -e ANSIBLE_VAR=ANSIBLE_VALUE
```
Where after __--__ going ansible arguments passing from molecule to your playbook:
 - become __-b__
 - any additional variables for role __-e ANSIBLE_VAR=ANSIBLE_VALUE__

> NB!!! Don't forget execute destroy after end of testing
```shell
molecule destroy
```

