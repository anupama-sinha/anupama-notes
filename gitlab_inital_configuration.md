## Steps
* git config --global user.name "Anupama Sinha"
* git config --global user.email "email-id"
* Generate file in Users/.ssh folder : ssh-keygen -o -t rsa -C "email-id" -b 4096
* Copy SSH & paste in Gitlab/Settings/SSH Key : cat ~/.ssh/id_rsa_pub | clip
