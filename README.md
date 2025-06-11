# minecraft-aws-iac

# Minecraft AWS Automation Pipeline

## Background

We fully automate the provisioning and configuration of a Minecraft: Java Edition server on AWS using Terraform (in PowerShell) for infrastructure and Ansible (in WSL) for server setup. This lets you deploy, configure, and restart the server without touching the AWS Console.

## Requirements

* **AWS CLI v2** with valid credentials (Access Key ID, Secret Access Key, Session Token)

  * Run `aws configure` in PowerShell to set them.
* **Terraform v1.5+** installed in PowerShell
* **WSL (Ubuntu)** with:

  * `ansible` installed (`sudo apt install ansible`)
  * `nmap` installed (`sudo apt install nmap`)
* **SSH key pair** (`minecraft.pem`) placed in:

  * Windows: `C:\Users\Shwin\Downloads\minecraft.pem`
  * WSL: copied to `~/.ssh/minecraft.pem` with `chmod 600`

## Commands to Run

1. **Terraform in PowerShell**

   cd terraform
   terraform init              
   terraform apply -auto-approve  

2. **Capture public IP** 

   52.71.120.187 = terraform output -raw public_ip

3. **Copy SSH key into WSL and set permissions**

   cp /mnt/c/Users/Shwin/Downloads/minecraft.pem ~/.ssh/
   chmod 600 ~/.ssh/minecraft.pem

4. **Run Ansible in WSL**

   cd /mnt/c/Users/Shwin/OneDrive/Desktop/Minecraft\ Part2/minecraft-aws-iac/ansible
   ansible-playbook -i hosts.yml playbook.yml

5. **Verify service**

   ssh -i ~/.ssh/minecraft.pem ec2-user@52.71.120.187 \
     "sudo systemctl status minecraft --no-pager"

6. **Test port open**

   nmap -sV -Pn -p T:25565 52.71.120.187


## Connect to the Server

* **Minecraft Java Edition**:

  1. Multiplayer → Add Server
  2. Server Address: `52.71.120.187`

## Sources

- https://developer.hashicorp.com/terraform/tutorials/aws-get-started
- https://developer.hashicorp.com/terraform/tutorials
- https://spacelift.io/blog/terraform-tutorial
- https://docs.ansible.com
- https://www.geeksforgeeks.org/using-ansible-documentation-effectively
- https://docs.aws.amazon.com/cli/latest/userguide/getting-started-quickstart.html
- https://www.youtube.com/watch?v=Qfg6hRY4Tq0
