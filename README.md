# EC2 Deployment using Ansible Playbook

This guide outlines the steps to deploy, stop, start, and terminate Amazon EC2 instances using Ansible playbooks. Please follow these steps carefully to ensure successful execution.

**Note:** Ansible commands and resource references can be found using `ansible-doc` in the command line or by searching for the relevant module.

## Prerequisites

- An EC2 instance to act as the Ansible node.
- Access and secret keys for your AWS account.
- Putty for connecting to the EC2 instance.

## Deploy an EC2 Instance

1. Connect to your EC2 instance via Putty.
2. Log in as root using `sudo su -l`.
3. Install Ansible: `sudo amazon-linux-extras install ansible2`.
4. Install Python package manager: `yum install python-pip`.
5. Install AWS SDK for Python (boto): `pip install boto`.
6. Create a `.vimrc` file for Vim settings and add `:set nu` to display line numbers.
7. Create an Ansible playbook file for EC2 deployment, e.g., `london.yml`.

```yaml
---
- name: Ansible Play
  hosts: localhost
  tasks:
    - name: Launching AWS instance using Ansible
      ec2:
        aws_access_key: (Access Key)
        aws_secret_key: (Secret Key)
        key_name: key
        region: eu-west-2
        instance_type: t2.micro
        image: ami-05c8ca4485f8b138a
        instance_tags:
          Name: Daemo_server
```

8. Check syntax errors: `ansible-playbook --syntax-check london.yml`.
9. Perform a dry run: `ansible-playbook -C london.yml`.
10. Run the playbook: `ansible-playbook london.yml`.
11. Check the AWS Management Console for the newly created instance.

## Manage EC2 Instances

### Stop Instances

1. Create a playbook file for stopping instances, e.g., `stop.yml`.

```yaml
---
- name: Stop instances
  hosts: localhost
  tasks:
    - name: Stop instances that were previously launched
      ec2:
        aws_access_key: (Access Key)
        aws_secret_key: (Secret Key)
        state: stopped
        region: eu-west-2
        instance_tags:
          Name: Daemo_server
```

2. Check syntax: `ansible-playbook --syntax-check stop.yml`.
3. Run the playbook: `ansible-playbook stop.yml`.
4. Verify instance state in the AWS Management Console.

### Start Instances

1. Create a playbook file for starting instances, e.g., `start.yml`.

```yaml
---
- name: Start instances
  hosts: localhost
  tasks:
    - name: Start instances that were previously stopped
      ec2:
        aws_access_key: (Access Key)
        aws_secret_key: (Secret Key)
        state: running
        region: eu-west-2
        instance_tags:
          Name: Daemo_server
```

2. Run the playbook: `ansible-playbook start.yml`.
3. Verify instance state in the AWS Management Console.

### Terminate Instances

1. Create a playbook file for terminating instances, e.g., `terminate.yml`.

```yaml
---
- name: Terminate instances
  hosts: localhost
  tasks:
    - name: Terminate instances that were previously created
      ec2:
        aws_access_key: (Access Key)
        aws_secret_key: (Secret Key)
        state: absent
        region: eu-west-2
        instance_ids: (Obtain instance ID from Management Console)
```

2. Run the playbook: `ansible-playbook terminate.yml`.
3. Verify instances have been terminated in the AWS Management Console.

Please exercise caution when using these playbooks, as they manipulate EC2 instances in your AWS environment. Always double-check settings and configurations before executing them.
