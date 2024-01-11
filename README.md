
# Aws EC2 Key File Workaround

This repository contains information on how to regain access to an EC2 instance in the event that the key file is lost.



    The simplest way is to create a AMI of the instance and creating a new instance with a new key using the AMI


## Steps

Consider Having a Ubuntu instance called **Instance-A** and you have lost its key (.pem file)

- create a new instance with same region and VPC as of the lost pem file instance. Name it as **helper** instance and create a new Key file for this (**helperkey.pem**).

- Now stop the **Instance-A** file instance. *Remember not to terminate instance but to stop it*

- Go to EBS volumes, select the root volume of the **Instance-A** and detach.

- Attach the **Instance-A** volume to the **helper**
(sdf) instance

- Now start and Login to the **helper** instance and Mount the Attached Volume to a Folder in the Root volume 

**check for the attached volume**


    lsblk


**Mount it**

    mkdir /mnt/root-vol
    mount /dev/xvdf1 /mnt/root-vol

**check it** 

    df -h

- Copy the SSH auth keys to the attached Volume

    cat /home/ubuntu/.ssh/authorized_keys >> /mnt/root-vol/home/ubuntu/.ssh/authorized_keys

**unmount it**

    umount /mnt/root-vol/


- Now Detach this attached volume and re-attach to the **Instance-A**

- Now login with the **helper** key 
