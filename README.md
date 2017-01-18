# ec2-restart
Ansible play to restart instance and attach to existing EIP and ELB
The objective is cleanup previous instance if it's around, and start a new instance.
Need to reuse an existing EIP, there is a need to maintain ssh connectivity without relying on
DNS changes.
Same goes for the ELB, we could dynamically recreate the ELB, but instead we want to keep existing
external IP address. We attach the newly created instance to the existing ELB, albeit using aws
command ( Until boto3 support is added to Ansible, and ELBv2 is fully supported ). 

# Assumptions
- Env vars AWS_ACCESS_KEY_ID and AWS_SECRET_ACCESS_KEY are exported with AWS access key
- export ANSIBLE_HOST_KEY_CHECKING=False # Instance will keep the same EIP, host checking is problematic
- SSH private keypair is is ~/.ssh/id_rsa ( or equivalent ). Required to run a post config on instance
- Edit vars/ec2_vars.yml accordingly

# 
