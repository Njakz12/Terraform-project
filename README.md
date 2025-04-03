%% AWS Infrastructure Diagram (Terraform Deployment)
graph TD
    subgraph AWS VPC
        VPC["VPC (10.0.0.0/16)"]
        IGW["Internet Gateway (IGW)"]
        VPC --> IGW

        subgraph Public Subnet 1 (us-east-1a)
            Subnet1["Subnet (10.0.0.0/24)"]
            EC2_1["EC2 Instance #1<br>(userdata.sh)"]
            Subnet1 --> EC2_1
        end

        subgraph Public Subnet 2 (us-east-1b)
            Subnet2["Subnet (10.0.1.0/24)"]
            EC2_2["EC2 Instance #2<br>(userdata1.sh)"]
            Subnet2 --> EC2_2
        end

        ALB["Application Load Balancer (ALB)"]
        ALB --> EC2_1
        ALB --> EC2_2

        SG["Security Group (SG)<br>Rules:<br>- HTTP (80)<br>- SSH (22)"]
        EC2_1 --> SG
        EC2_2 --> SG

        S3["S3 Bucket<br>(njakz-bucket)"]
    end

    User["User"] -->|HTTP Traffic| ALB
    ALB -->|DNS Output| ALB_DNS["ALB DNS: myalb-123456789.us-east-1.elb.amazonaws.com"]


This Terraform project automates the deployment of a highly available and scalable web infrastructure on AWS. It includes:

A VPC with public subnets in different availability zones.

EC2 instances running a web application.

- An Application Load Balancer (ALB) to distribute traffic.

- A security group allowing HTTP (port 80) and SSH (port 22) access.

- An S3 bucket for storage.

