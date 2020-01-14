# Case
Provides an Ansible playbook to install Chocolatey, git and OpenSSH, also configure private key access SSH on a windows 10 host, using WinRM - ntlm - http on a local network.


## Prerequisite
### Controller

The playbook should run from a linux controller running :
Ansible 2.8 **with** Python 3.7 and pywinrm module should be installed `pip install "pywinrm>=0.3.0"`

### The Windows Host

Windows 10-1910
The OpenSSH version is 8.0.0.0
.Net Framework 4.8 +
Powershell 5.1 +

### Windows Host Credentials
The Scripts have been run and tested using Admin user on the host machine

## Configure the inventory

In the inventory file `./inventories/case_study/hosts.yml` (see image below)
![Inventory Configuration](https://i.imgur.com/fS00jDA.png)
1. Specify the hostname or IP Address
2. Specify the User / Password. Note that it is a better practice to use [Ansible Vault](https://docs.ansible.com/ansible/latest/user_guide/vault.html) rather than putting passwords in clear.  
3. Specify winrm Transport (could be basic/certificate/ntlm/kerberos/credssp) see documentation of [Windows Remote Management](https://docs.ansible.com/ansible/latest/user_guide/windows_winrm.html)

## Configure the public key to be uploaded to the host machine

### Public Key to be uploaded
Modify the file `./vars/upload_ssh_pub_key.yml` and copy/paste your public key between the quotes.

### Authorized_Keys
Configure the path for the Authorized_Keys store on the host machine
The paths in the `./upload_ssh_pub_key.yml` playbook are the default paths for a windows 10 administrator user.

## How to execute this Playbook

Using the command line, after cloning the repository, to your control machine from the root path of the repository launch the following line.

`ansible-playbook -i ./inventories/case_study/hosts.yml casestudy.yml`

## Azure provisioning

in `provisioning_azure_vm\createvm.yml` provisions an Azure Windows VM to run it simply run

`ansible-playbook createvm.yml`

To run the previous script against the newly created VM update the static repository in `inventories/azurevm/hosts.yml` with the correct value and run the script against that repository. Make sure the WinRM listener is listening on the public Interface of the windows VM.

`ansible-playbook -i ./inventories/azurevm/hosts.yml casestudy.yml`
