# ec2-restart
Ansible playbook to reinitialize EC2 instance and attach to existing EIP and ELB.
The objective is to cleanup previous instance(s) if any around, and start new ones.
Need to reuse an existing EIP, there is a requirement to maintain connectivity without 
the risk of delays introduced by DNS changes.

Same goes for the ELB, we could dynamically recreate the ELB, but instead we want to keep the
existing external IP address. We attach the newly created instance to the existing ELB, albeit using aws
command ( Until boto3 support is added to Ansible, and ELBv2 is fully supported ). 

# Assumptions
- Env vars AWS_ACCESS_KEY_ID and AWS_SECRET_ACCESS_KEY are exported with required AWS access key
- export ANSIBLE_HOST_KEY_CHECKING=False # Instance will keep the same EIP, host key checking is problematic
- Defaulted to start a single instance per AZ. Ultimatly, we want to support multiple AZ, but without loosing
  all instances at once. Rolling restarts can be accomplished using different variables across multiple runs.
- SSH private keypair is available @ ~/.ssh/id_rsa ( or equivalent ). Required to run a post config on instance
- Edit vars/ec2_vars.yml accordingly

# Sample Run

\$ansible-playbook ec2_setup.yml --extra-vars "varfile=/home/myhome/otherAZ_vars.yml 

# Wish list
- Support single playbook for rolling restarts across Availibility Zones
