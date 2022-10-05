# Creating a JupyterLab Server on a cloud VM

## Requirements
1. An Amazon Web Services Account (AWS)
2. AWS Credits

## Getting Started
1. Log in to the AWS management console https://aws.amazon.com/.
2. Once logged in, search 'EC2' in the search bar and click on the first result.
![plot](./images/aws-launch-instance.png)
3. Name your instance and select Ubuntu 18.04 LTS as the VM's OS.
![plot](./images/ubuntu-selection.png)
4. Select your instance type based on the number of users that will be utillizing the server.  
   Note: For this conference we used a t3.2xlarge VM but that may be too much for a smaller setup.
![plot](./images/instance-type-selection.png)
5. In the networking section allow HTTP, HTTPS, and SSH traffic from anywhere.
![plot](./images/network-settings.png)
7. Finally, add the following in the User Data section in the Advanced details section. 
```
Content-Type: multipart/mixed; boundary="//"
MIME-Version: 1.0

--//
Content-Type: text/cloud-config; charset="us-ascii"
MIME-Version: 1.0
Content-Transfer-Encoding: 7bit
Content-Disposition: attachment; filename="cloud-config.txt"

#cloud-config
cloud_final_modules:
- [scripts-user, always]

--//
Content-Type: text/x-shellscript; charset="us-ascii"
MIME-Version: 1.0
Content-Transfer-Encoding: 7bit
Content-Disposition: attachment; filename="userdata.txt"

#!/bin/bash
curl -L https://tljh.jupyter.org/bootstrap.py \
  | sudo python3 - \
    --admin [admin-username]
```
9. Create your instance
