---
title: Unlock the Power of the Cloud - Deploy Your Django App on AWS EC2
description: You’re happily coding away on your Django Web App, and now you’re ready to deploy it in a production cloud environment. But the transition from local development to a robust, secure, and scalable production setup can be challenging. You want your app to be easily accessible to clients, have robust security for malicious actors all while hosted on a cloud provider with a reliable architecture that will give you peace of mind. While Django is good for handling all your Web App needs, moving from local to a production environment will certainly present new challenges.
date: 2024-11-04
author: deniel
categories: [Cloud Computing]
tags: [Amazon EC2, Django]
pin: false
image: 
  path: /assets/img/deploy-your-django-app-on-aws-ec2/banner.jpg
---

You’re happily coding away on your Django Web App, and now you’re ready to deploy it in a production cloud environment. But the transition from local development to a robust, secure, and scalable production setup can be challenging. You want your app to be easily accessible to clients, have robust security for malicious actors all while hosted on a cloud provider with a reliable architecture that will give you peace of mind. While Django is good for handling all your Web App needs, moving from local to a production environment will certainly present new challenges.

## AWS as a Cloud Service Provider

AWS is one of those cloud service providers. They certainly achieve all our checklists for moving on a production environment with their various services and global network of data centers. It offers the scalability to handle varying user demands, the security tools to protect user data, and the reliability needed for high availability. For developers looking to move from local to cloud, AWS provides the infrastructure and flexibility to make the transition smooth and sustainable.

## Harboring the power of AWS EC2

AWS EC2 or Elastic Compute Cloud is a really practical and widely used service for hosting web apps, offering on-demand, scalable computing capacity. With EC2, you can minimize upfront hardware costs and focus on developing and deploying your application faster. Its flexibility in instance types and configurations allows you to scale resources as needed, making it an ideal choice for supporting a growing user base in a reliable and cost-effective manner.

AWS Elastic Compute Cloud offers a wide range of features that will help you deploy a future proof application.  

- **Elasticity** is one of its main features, that allows you to configure an **Auto Scaling Groups** (ASGs) to automatically adjust capacity in response to changing traffic loads, ensuring that your app performs reliably even during high traffic situations.
- EC2 also prioritizes **security with Security Groups** and **Virtual Private Clouds** (VPCs), providing network-level protection and controlled access to your instances, which helps safeguard user data and sensitive app components.
- For **cost management**, EC2’s flexible pricing model—spanning on-demand, reserved, and spot instances—lets you optimize costs based on your application’s needs, fitting various budget constraints.
- Finally, **EC2 integrates seamlessly with the broader AWS ecosystem**, offering easy access to services like RDS for databases, S3 for storage, and IAM for access control, enabling a cohesive, scalable, and secure setup that covers all the essentials for production-ready Django deployments.

---

# Launching an EC2 Instance

In the AWS Management Console, type `EC2` in the search bar or locate it under the **Services** menu. Click on **EC2** to open the EC2 Dashboard.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/clwcacdxau1rtjzlg5we.png)

#### 1. Launching the Instance

On the EC2 Dashboard, click the **Launch Instance** button to start the instance creation process.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/1h0hxgu1ghjn6pmp19n9.png)

#### 2. Naming and Tagging Your Instance  

Create a name for your instance.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/vxufom1199ul8fds0vnr.png)

#### 3. Selecting an AMI (Amazon Machine Image)

AMIs are preconfigured templates for your instances, containing the operating system and software you want. Choose ‘Amazon Linux’.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/0jmpslmk60j3u3x99p22.png)

#### 4. Choosing an Instance Type

An instance type in Amazon EC2 defines the virtual hardware for your server, such as CPU, memory, storage, and network capacity. Different types are suited for different workloads.

- **Choose t2.micro** for this one as it's affordable, Free Tier eligible, and offers on-demand usage where you only pay for what resources you use.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/ht2rlxikn74l25ns7g3t.png)

#### 5. Configuring a Key Pair

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/2kdd5wz1y8e9uos7moem.png)

For this time, lets not create a keypair. We will connect to the instance using the AWS console.

#### 6. Configuring Networking

These are some minute networking details for your instance.

- **Network:** The network defines the Virtual Private Cloud (VPC) where your instance will reside.
- **Subnet:** A subnet is a segment of a VPC's IP address range.
- **Auto-assign Public IP:** This setting automatically assigns a public IP address to your instance.
  
![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/h1h267yto6p37gnsfj83.png)

#### 7. Configuring Security Groups

Security groups act as virtual firewalls for your Amazon EC2 instances. They control inbound and outbound traffic based on defined rules. Choose **create security group** and check **allow SSH traffic**. This allows us to connect to our instance later.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/9p98q57viwxv276r8umx.png)

#### 8. Configuring Storage

This will specify the storage options of your instance. Leave the option as it is. Here are the details you need to know:

- **1x:** This indicates that there is one volume attached as the root volume to the EC2 instance.
- **8 GiB:** This specifies the size of the root volume, which is 8 GiB (Gibibytes).
- **gp3:** This indicates the type of storage volume. gp3 is a type of General Purpose SSD (Solid State Drive) that offers a balance of price and performance.
  
![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/8hobvebcplc4vpsepkem.png)

#### 9. Reviewing and Launching

Review your configured instance. If everything looks correct, click **launch instance**.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/wra17gasq39gn64xwlus.png)

#### 10. Accessing the Instance

You will proceed to this page, just click the instance ID.  

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/4bizwlgdtzt347n9bcpg.png)

The instance will take time to initialize. It will usually take 2-3 minutes.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/s664lzjhfm2a4zk804q5.png)

---

## Django on a Git Repository

Now that you have your EC2 instance running, you can now deploy your Django application. We will clone it from a remote repository.

[Link to remote repo](https://github.com/euxia/django-on-ec2)  

This remote repo is not made by me, all credits to the author. He also had a guide on how to deploy Django on EC2 in his README.md file. you can check it out. His guide is on Ubuntu, but we will be using Amazon Linux.

## Deploying Django on EC2

### Connecting to the instance

- Click the instance ID to open the instance details. Then click **Connect**.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/08qmair82iyst0xv6g7x.png)

- Lets choose EC2 Instance Connect as the connection method and click **Connect**. This is due to the fact that we did not create a keypair and it is much easier to connect using this method.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/a0y7ipnoke44xiydrxoz.png)

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/pln61tgpmzblbrr9gmar.png)

### Configuring Django dependencies on the instance

- Update the package list system

```bash
sudo yum update -y
```

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/n30jqd6adyd5308phclp.png)

- Lets install git

```bash
sudo yum install git 
```

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/gik7z88nn05ncr8b902c.png)

- Clone the repository

```bash
git clone https://github.com/euxia/django-on-ec2
```

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/nr5y4i5x7knoz9k5uoqt.png)

- Install Python3 and Django

```bash
sudo yum install python3-pip -y
```

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/qy8ltpnm49e73btbpknq.png)

```bash
pip install django
```

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/3ext4uxszb8ia3nhx0an.png)

---

### Go to the repository and run the Django server

- Go to the repository

```bash
ls
```

```bash
cd django-on-ec2
```

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/t377idco1n3fsi9ooqi2.png)

- Lets create a migration. This prepares the changes in your models to be applied to the database

```bash
python3 manage.py makemigrations
```

```bash
python3 manage.py migrate
```

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/fvk7eukw2cxq0953s2wj.png)

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/wl1ols2c53dfr4hon2il.png)

- Lets create a superuser. This is the admin user for the Django application.

```bash
python3 manage.py createsuperuser
```

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/pdj2aax0sfdfl54csaeb.png)

*Tip: You can skip the email part if you want.*

- Run the Django server

```bash
python3 manage.py runserver
```

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/6cgrj6kparenrc36vn60.png)

As you can see, the server is running on a local IP address. But we can't access it from the internet as it is running on a production server. We will need to run it on a public IP address of the EC2 instance itself.

---

### Running Django on a public IP address

- Stop the server by pressing `Ctrl + C`

- Run the server on a public IP address

```bash
python3 manage.py runserver 0.0.0.0:8000
```

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/iu636ot3crqxdpz8z4n5.png)

This will run the server on a public IP address accessible from everywhere, on all network interfaces.

- Find the public IP address of the instance then add `:8000` to the end of the IP address.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/s7d3g95yf63ftsk78gop.png)

Search the public IP address on your browser and you will see the Django application still not running.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/ynnpq7z9k5m1qkvcj9wb.png)

***

### Allowing traffic on port 8000

- Go to the EC2 instance, scroll down, and find the security group. Click on the security group ID.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/dncfn06f094azpypmnqe.png)

- Click on the **Inbound rules** tab and click **Edit inbound rules**.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/97gkyfqwo1e9j3w9lqah.png)

Click **Add rule** and add the following details:

- **Type:** Custom TCP Rule
- **Protocol:** TCP
- **Port Range:** 8000
- **Source:** Anywhere

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/poylf23sf3ogxi59bwhz.png)

- Click **Save rules**.

- Go back to the browser and refresh the page. You will see the Django application running.

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/5jgwyowvgxaz6dg72d6b.png)

---

Deploying a Django web app on AWS EC2 illustrates the power and versatility of cloud infrastructure—not just for Django, but for a wide variety of web frameworks and languages. The steps involved, such as configuring inbound rules to allow specific port permissions and setting up security groups, can be applied equally to applications built with Flask, Node.js, and even frontend frameworks like Next.js.
