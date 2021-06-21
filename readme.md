adduser lukas

passwd lukas

vi /etc/ssh/sshd_config

PasswordAuthentication yes

systemctl stop sshd

systemctl start sshd

visudo

## Allow root to run any commands anywhere
lukas ALL=(ALL) ALL

ansible-playbook remote-symb-link.yml -K