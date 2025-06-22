#ssh 

`ssh-keygen -t rsa -b 4096 -f ~/.ssh/vyos_monitoring_key`

How to Use chmod for SSH Keys
Here’s how to set the correct permissions for your SSH keys and the .ssh directory:

Set directory permissions (owner can read, write, and execute; no access for others):
`chmod 700 ~/.ssh`

Set private key permissions (owner can read and write; no access for others):
`chmod 600 ~/.ssh/vyos_monitoring_key`

Set public key permissions (owner can read and write, others can read):
`chmod 644 ~/.ssh/vyos_monitoring_key.pub`