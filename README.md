#  Part 2 - Jenkins Infrastructure

This repository sets up an Jenkins `EC2` instance using the `AMI` that is built in Part 1. Fill the values for the following variables based on your AWS profile and elastic IP you want to associate. Once the values have been populated, create a private network and instance using the ansible commands listed below.

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

Run the ansible playbook below to setup/teardown networking in the AWS account as mentioned by the AWS profile in variables which points to the config and credentials on the local system.

### Setup

```bash
ansible-playbook jenkins-networking-setup/setup-networking.yml --extra-vars "@extra_vars_jenkins.json" -vvv
```

### Teardown

```bash
ansible-playbook jenkins-networking-teardown/teardown-networking.yml --extra-vars "@extra_vars_jenkins.json" -vvv
```

## Instance

Run the following playbook after the networking setup is done. Change the argument `--private-key` in the below command which points to the path of the SSH key used in the variables above. This command creates an `EC2` instance using the custom Elastic IP and custom AMI Id.

### Setup

```bash
ansible-playbook jenkins-instance-setup/setup-instance.yml --private-key "~/.ssh/aws-mac" --extra-vars "@extra_vars_jenkins.json" -vvv
```

### Teardown

```bash
ansible-playbook jenkins-instance-teardown/teardown-instance.yml --private-key "~/.ssh/aws-mac" --extra-vars "@extra_vars_jenkins.json" -vvv
```

## Jenkins Configuration

Once the instance is up and running, its accessible on the browser at `<<Elastic IP Address>>:8080`. Following is the configuration required for the pipelines.

- Login into jenkins as admin using the following way -  https://stackoverflow.com/a/46842541/3884734
- Create an account for user (optional)
- Go to `Manage Jenkins` and create 4 credentials which are used in the pipeline as listed below
    - `github-sajal` which is a username/password type credential with user's github username and password
    - `github-sajal-token` which is a secret text type credential with user's github personal access token
    - `sajal-dockerhub` which is a username/password type credential with user's docker username and password
    - `backend_server_url` which is a secret text type credential which points to the Load Balancer URL of the backend service running on k8s 


