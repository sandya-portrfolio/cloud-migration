**Automated VM Migration to AWS using VM Import/Export**

**Problem Statement:**
Migration on-prem VMs to AWS manually is error-prone and time-consuming. This project automates VM Migration using AWS VM Import/Export with S3 and CLI

Architecture Overview:
Source: On-prem (VMware/Hyper-V)
Target: AWS EC2 (AMI)



**Prerequisite:**

* Download, install and configure AWS Command Line Interface --https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html
* vmimport - VM Import Service Role --https://docs.aws.amazon.com/vm-import/latest/userguide/required-permissions.html

1. Export VM from its current environment as an OVA file (or VMDK, VHD, or RAW) (if you are testing this make sure the VM has at least one user configured with password)
2. Create a S3 bucket and upload the VM image to S3 using upload / drag and drop, or using AWS CLI
3. Import your VM using the ec2 import-image command
4. Use the ec2 describe-import-image-tasks command to monitor the import progress
5. Once Import is completed Launch EC2 Instance from the AMI created, or copy the AMI to other region

**Step by Step process:**
Shut down the Mechine(win 7) on VMware workstation
1.1. Export to VM formate(OVA, VMDK, VHD, RAW)  
   VMware workstation--file--Export to OVF--option to chose formate of file--choosen OVA formate(name of file: "Windows 7 for Demo.ova") since around 20GB it will take arount 15min time.
2.1. Create S3 bucket and upload Windows 7 for Demo.ova file
3.1. Create VM Import Service Role   save "trust-policy.json" (gave "" so that it get saved in the .json format) filepath: file://C:migration/trust-policy.json
{
   "Version":"2012-10-17",
   "Statement": [
      {
         "Effect": "Allow",
         "Principal": { "Service": "vmie.amazonaws.com" },
         "Action": "sts:AssumeRole",
         "Condition": {
            "StringEquals":{
               "sts:Externalid": "vmimport"
            }
         }
      }
   ]
}

3.2.  create role by using below command
      aws iam create-role --role-name vmimport --assume-role-policy-document "file://C:migration/trust-policy.json"
      This command will create role in your mechine from where you start migration 
      




