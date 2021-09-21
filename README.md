# Chat-App-using-Socket

A simple chat app
  
## How to deploy and run Node js app Using cloudformation

### Step 1 (Create stack)

 1. You need to go to the CloudFormation file You will be able to see 3 files
    * networking.yaml
    * load-balancer.yaml
    * autoscalling.yaml

    Simply go to your AWS account and search **Cloudformation** in the search bar then create a stack click on `With new resources (Standard)` then is Specify **template** select `Upload template file`

 2. To create the stacks you need to go by order as this list down bellow
    * (First) networking.yaml
    * (Second) load-balancer.yaml
    * (Third) autoscalling.yaml

In the Autoscalling.yaml you can specify instances type and the Key Pair if your going with default one you should download the Key pair i am using in the link down below

### Step 2 (Run asnible playbook)

In order to deploy the app on the autoscalling groups first we need to connect to an Jump host server from the server we can connect to the servers

1. Before running the ansible playbook you need to copy the key pair from your local machine to the jump host

    * Download the key Pair from here [CFM.pem](https://drive.google.com/file/d/1hZUsyhsJtLKTFNXotawDBaXQqewssJee/view?usp=sharing)
    * To get the IP address of the Jump host and the autoscalling group follow this step
        * In the ec2 instances search for jumphost and copy the public ip address
        * In the ec2 target group ChatTarget in the Target section see the following instances and copy the IP address
    * Go to Playbook in the Chat-app-using-socket copy the IP address of the servers

    ```yaml
    server ansible_ssh_host=10.0.2.244 ansible_user=ubuntu 
    Jump-host ansible_host=52.91.88.215 ansible_connection=ssh ansible_user=ubuntu
    ```

    copy the IP address in the ansible_host and ansible_ssh_host
    or add more server that you want  

    * To copy the keypair from your local machine to the jump host server run this comand
    `sudo scp -i CFM.pem CFM.pem ubuntu@52.91.88.215:/home/ubuntu`

2. Run the ansible play book for the jumphost
    * `sudo ansible-playbook -i inventory jumphost.yaml --key-file CFM.pem`
        If the ansible ask you when try to connect to the server type yes
    * After is complete ssh to the server using this command
      `sudo ssh -i CFM.pem ubuntu@52.91.88.215`

3. Run the ansible play book for the servers
    * When you inside the jump host you need to run the ansible play book first you need to go inside the chat/Playbook directory

    * Run this command for the ansible playbook
     `sudo ansible-playbook -i inventory install.yaml --key-file CFM.pem`

### Step 3 (Check and Present Route 53)

1. For checking the app if it is running you need to go to the AWS EC2 load-balancer copy the DNS name and paste it in the browser if you see the app running it means task work successfully

2. To present a domain name for your server copy the load-balancer DNS name the go to the route 53 service

    * Choose any Hosted domain that you all ready have then created a record
        * Record type: CNAME
        * Value: Copy the load-balancer DNS name
        * Record name: Write the name that you want to associate with the domain that you choose

## Congratulations you have deploy the app
