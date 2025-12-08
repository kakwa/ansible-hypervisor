# ansible-hypervisor
ansible code for my homelab/hypervisor

## Quick Bootstrap

Before running the playbook, bootstrap the target machine with SSH access and sudo privileges:

```bash
# Set variables
export HOSTNAME=your-hostname
export LOGIN=kakwa

# Copy SSH key to target
ssh-copy-id $LOGIN@$HOSTNAME

# SSH and setup sudo using su
ssh $LOGIN@$HOSTNAME "su -c '
apt update && apt install -y sudo
/usr/sbin/usermod -aG sudo $LOGIN
echo \"$LOGIN ALL=(ALL) NOPASSWD:ALL\" > /etc/sudoers.d/$LOGIN
chmod 440 /etc/sudoers.d/$LOGIN
'"

# Run the Ansible playbook
ansible-playbook -i inventory hypervisor.yml
```
