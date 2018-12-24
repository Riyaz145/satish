Create EC2 instance:
--------------------------------------------------
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

-------------------------------------------------------------------------------------------------------
-------------------------------------------------------------------------------------------------------
SSH connection to server:
-------------------------------------------------
Download PuTTY from http://www.chiark.greenend.org.uk/~sgtatham/putty/ or another PuTTY download source. The "putty.exe" download is good for basic SSH.
Save the download to your C:\WINDOWS folder.
Double-click on the putty.exe program to launch the application.
Enter your connection settings:
      (a)Host Name (or IP address): 18.216.69.169 
      (b)Port: 22 (default)
      (c)Connection Type: SSH --->select Auth---> Browse your ppk file --->open
      
---------------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------------
Install Ansible on Ubuntu 16.04
---------------------------------
$ sudo apt-get update
$ sudo apt-get install ansible -y
To remove ansible: $ sudo ap-get remove ansible
Configure Ansible Hosts & Groups: $ cd /etc/ansible/
$sudo  v

