# Ansible - AWS Gaming PC

These scripts are provided without any warranty.  Use them at your own risk!

## Playbooks

### Create
This playbook will spin up a small VPC with a subnet, security group and internet gateway and then launch a *spot* instance request.

The instance will have Chrome, Steam and Parsec installed on it.  Parsec will be installed in shared user mode.

The instance will have a main volume for the OS and an extra volume intended for the games.  The main volume is configured to be deleted on instance termination.

```shell
ansible-playbook -D create.yaml
```

### Get Info
This playbook will print out the public DNS for the instance after it's running.

```shell
ansible-playbook -D getinfo.yaml
```

### Stop
This playbook will stop the instance, snapshot the volumes and then delete them.

```shell
ansible-playbook -D stop.yaml
```

### Start
This playbook will reattach the volumes from the snapshots created when it was stopped, start the instance and then delete the snapshots.

```shell
ansible-playbook -D start.yaml
```

## Usage

You will need to have [Ansible](https://github.com/ansible/ansible) installed.

Modify the variables in [vars/gaming.yaml](vars/gaming.yaml) according to your needs.

Create a variable file called `vars/private.yaml` and add the instance profile you want to use along with your home IP to that one.  This IP will be added to the security group.

```yaml
---
home_ip: 1.2.3.4/32
iam_instance_profile: arn:aws:iam::<account id>:instance-profile/<name>
```

Install the python requirements.

```shell
pip install -r requirements.txt
```

Install the ansible requirements.

```shell
ansible-galaxy collection install -r requirements.yaml
```

The script will use the credentials configured in your profile.  Please ensure you are running these scripts in the correct account!
