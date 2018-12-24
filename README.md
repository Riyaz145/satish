               # Create EC2 instance #
----------------------------------------------------------------------------------------------
               Open the Amazon EC2 console at https://console.aws.amazon.com/ec2/.
               Choose Launch Instance.
               Choose an Amazon Machine Image (AMI), find an Ubuntu Server 16.04 LTS AMI of the list and choose Select.
               Choose an Instance Type(t2 micro), choose Next: Configure Instance Details.
               Configure Instance Details[number of instances 2], choose Network, and then choose the entry for your default VPC.
               Choose Next: Add Storage.
               Choose Next: Tag Instance.
               choose Next: Configure Security Group:
                    (a) Assign a security group is set to Create a new security group
                    (b)Type: SSH 
                    (c)Protocol: TCP
                    (d)Port Range: 22 
                    (e)Source: Anywhere 0.0.0.0/0 
               Choose Review and Launch.
               Choose Launch.
               Select the check box for the key pair that you created, and then choose Launch Instances.
               Choose View Instances.
               Hostname of Instance 1 : MSR-test-Instance-1
               Hostname of Instance 2 : MSR-test-Instance-2

---------------------------------------------------------------------------------------------------
# SSH connection to server #
---------------------------------------------------------------------------------------------------
Download PuTTY from http://www.chiark.greenend.org.uk/~sgtatham/putty/ or another PuTTY download source. The "putty.exe" download is good for basic SSH.
Save the download to your C:\WINDOWS folder.
Double-click on the putty.exe program to launch the application.
Enter your connection settings:
      (a)Host Name (or IP address): 18.216.69.169 
      (b)Port: 22 (default)
      (c)Connection Type: SSH --->select Auth---> Browse your ppk file --->open
      
---------------------------------------------------------------------------------------------------
# Install Ansible on Ubuntu 16.04 #
---------------------------------------------------------------------------------------------------
$ sudo apt-get update
$ sudo apt-get install ansible -y
To remove ansible: $ sudo ap-get remove ansible
Configure Ansible Hosts & Groups: $ cd /etc/ansible/
$sudo vi hosts
add hosts
///[MSR-test-Instance-1] 
18.216.69.169
[MSR-test-Instance-2]
18.222.189.132///

[all]
18.216.69.169
18.222.189.132
 
 -------------------------------------------------------------------------------------------------
# command to generate key:
 $ ssh-keygen  -->enter 3 times 
 -->go to ssh   --> $ cd .ssh
 $ cat id_rsa.pub ---> copy public key and paste it on authorized_key ---> $ sudo vi authorized_key
 Repeat same step --copy  id_rsa.pub key from instance 1 and paste in authorized keys for instance 2
 give root permissions --->$ cd /etc/ssh/ ----> $ sudo vi sshd_config ----> passwordAuthentication yes
 restart ssh ----> $ sudo service sshd restart
 To check status ---> $ sudo service sshd status
 
 ---------------------------------------------------------------------------------------------------
# To create ansible playbook #
---------------------------------------------------------------------------------------------------
 $ cd /etc/ansible/ 
create playbook --->$ sudo vi playbook.yml

- hosts: all
  become: true
  tasks:

  - name: Install git
    apt: pkg=git
  
  - name: nodejs 8.12
    get_url:
        url: https://nodejs.org/dist/v8.12.0/node-v8.12.0.tar.gz
        dest: /opt

  - name: Extract node tar.xz
    unarchive:
     src: /opt/node-v8.12.0.tar.gz
     dest: /opt
     remote_src: yes

  - name: Rename
    command: mv /opt/node-v8.12.0 /opt/yls/node-v8

  - name: Add Docker GPG key
    apt_key: url=https://download.docker.com/linux/ubuntu/gpg
        
  - name: Add Docker APT repository
    apt_repository:
       repo: deb [arch=amd64] 
    https:https://github.com/docker/compose/releases/download/1.18.0/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose
 
  - name: Install list of packages
    apt:
     name: ['apt-transport-https','ca-certificates','curl','software-properties-common','docker-ce']
      state: present
      state: present
       update_cache: yes

  - name: Install Docker Compose.

    get_url:

        url: https://github.com/docker/compose/releases/download/1.23.2/docker-compose-Linux-x86_64

        dest: /usr/local/bin/docker-compose

        mode: 4777
