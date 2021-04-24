#  Jenkins Infrastructure

## Extra variable in JSON file

### extra_vars_jenkins.json

```json
{
    "aws_profile": "",
    "aws_region": "us-east-1",
    "ami_id": "ami-0e057bdeb73803a0b",
    "ssh_key_name": "",
    "instance_type_name": "t2.medium",
    "terminate_tag_key": "App",
    "terminate_tag_value": "jenkins",
    "elastic_ip": "54.164.79.68"
}
```

## Networking

### Setup

```bash
ansible-playbook jenkins-networking-setup/setup-networking.yml --extra-vars "@extra_vars_jenkins-local.json" -vvv
```

### Teardown

```bash
ansible-playbook jenkins-networking-teardown/teardown-networking.yml --extra-vars "@extra_vars_jenkins-local.json" -vvv
```

## Instance

### Setup

```bash
ansible-playbook jenkins-instance-setup/setup-instance.yml --private-key "~/.ssh/aws-mac" --extra-vars "@extra_vars_jenkins-local.json" -vvv
```

### Teardown

```bash
ansible-playbook jenkins-instance-teardown/teardown-instance.yml --private-key "~/.ssh/aws-mac" --extra-vars "@extra_vars_jenkins-local.json" -vvv
```

### 