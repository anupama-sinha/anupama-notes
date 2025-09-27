## Steps
* git config --global user.name "Anupama Sinha"
* git config --global user.email "email-id"
* Generate file in Users/.ssh folder : ssh-keygen -o -t rsa -C "email-id" -b 4096
* Copy SSH & paste in Gitlab/Settings/SSH Key : cat ~/.ssh/id_rsa_pub | clip

## Steps to clone Repo & Push Code
* git clone git@github.com:anupama-sinha/delivery-application.git
* cd delivery-application/
* echo "# delivery-application" >> README.md
* git add README.md
* git commit -m "Initial Commit"
* git push origin main
