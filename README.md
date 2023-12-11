<h1 align="center">AWS Networking 3 Tier Architecture Project</h3>

![](https://github.com/CTheTechGuru/AWS-Networking-3-Tier-Architecture/blob/main/images/C%20Diagram%20-%20Page%201(1).png?raw=true)





<!-- PROJECT Details-->
# About The Project
In the ever-evolving landscape of cloud computing, crafting a robust and scalable architecture is paramount to the success of modern applications. This project embarks on a journey to construct a resilient 3-tier architecture on Amazon Web Services (AWS), employing best practices to ensure optimal performance, reliability, and security.

#### Overview:
This AWS project is structured around the fundamental principles of three-tier architecture, dividing the application into distinct layers – the Web Tier, Application Tier, and Database Tier. Each layer plays a pivotal role in delivering a seamless and efficient user experience, and the design ensures a high level of fault tolerance and scalability.

#### Infrastructure Setup:

The foundational components of our AWS environment have been meticulously configured. A Virtual Private Cloud (VPC) forms the backbone, providing a secure and isolated space for our architecture. Internet Gateways facilitate external communication, while carefully crafted Route Tables ensure efficient traffic flow. Network Address Translation (NAT) Gateways add an extra layer of security, allowing our private instances to access the internet while maintaining a secure posture.

#### Resilient Design:

In the face of potential failures, our architecture remains resilient. Multiple Availability Zones (AZs) have been strategically employed to distribute resources, mitigating the impact of localized disruptions. Load balancing mechanisms enhance fault tolerance and ensure optimal resource utilization, guaranteeing a consistent user experience even during peak loads.

### Tier Details:
1. Web Tier - Presentation also the UI of our users to access our application (ex: www.amazon.com) 
2. Application Tier - Backend aka logic layer. This layer is acts as the medium between to two Web and database layer.(user requests and data storing) 
3. Database Tier - Responsible for storing, managing, and retrieving information. ( DynamoDB, NoSQL, AmazonRDS, MySQL )

With the current state of technology and its fast paced evolution, businesses strive to 
maintain resiliency as well as fault tolerance to handle work loads beyond measure. 
This is where the AWS makes it seamless to utilize cloud services on demand, opening up 
a plethora of options for building secure, highly available, reliable, and performance efficient applications.  

 # Prerequisites


* Basic understanding of Computer Networks.
* Basic understanding of TCP/IP.
* Basic understanding of virtualization.
* Access to AWS (Free Tier or Paid)
* _Best Practice_ Create AWS user account with administrator access.(enable MFA on lab account)
* Download Putty and Pageant

##  Create VPC | Subnets | Route Table | IGW

### 1. Create VPC

* Open the VPC dashboard and on the left column choose Your VPC. 
* Give your VPC a name tag to identify. 
* Create a CIDR range for your VPC. Mines will be 10.16.0.0/16





![](https://github.com/CTheTechGuru/AWS-Networking-3-Tier-Architecture/blob/main/images/Create%20VPC.png?raw=true)

* When done click create to to build your vpc.  
* By default a route table, dhcp option set, nacl, and one security group.
* We will modify these later in the project to enhance security as we add more components. 



### 2. Create Subnets - We will create a subnets for our different instances

* We will create our subnets now, this is where networking knowledge comes in handy.  
* Our Cidr is 10.16.0.0/16, we will create a subnet per environment (WEB, APP, DB).
* On the main VPC dashboard look on the leftr column and choose subnets and create subnets on the next page.
* This is an example of our first subnet.
  
![](https://github.com/CTheTechGuru/AWS-Networking-3-Tier-Architecture/blob/main/images/Subnet%20Settings.png?raw=true)

* For high availability our architecture will be spread across multiple availability zones, create the remainder of subnets.

![](https://github.com/CTheTechGuru/AWS-Networking-3-Tier-Architecture/blob/main/images/Subnets.png?raw=true)
  

### 3. Create Internet Gateway and Route Table  

* In order to gain access to the internet we need to create a an internet gateway and routing table to access the internet.
* From there VPC main dashboard on the left column choose internet gateways, name and click create.
* Now highlight the internet gateway we've created and choose the actions tab above and Attach to VPC.
* Click the drop down and select attach.

![](https://github.com/CTheTechGuru/AWS-Networking-3-Tier-Architecture/blob/main/images/Attach%20IGW.png?raw=true)

* Now we will create a routing table and edit routes to add 0.0.0.0/0 as a destination, which will allow us to connect outside of our VPC to the internet.

* Now on the left column of the dashboard choose route tables. 
* Name the routing table and select the VPC you created.
* 
![]( https://github.com/CTheTechGuru/AWS-Networking-3-Tier-Architecture/blob/main/images/Route%20Table%20Internet.PNG?raw=true)

* Once the route table is created you will need to select edit the route to allow the IGW to have internet access.


Destination   | Target 
------------- | -------------
0.0.0.0/0     | IGW(Your Internet Gateway)


![](https://github.com/CTheTechGuru/AWS-Networking-3-Tier-Architecture/blob/main/images/Edit%20Routes.PNG)
![](https://github.com/CTheTechGuru/AWS-Networking-3-Tier-Architecture/blob/main/images/Route%20for%20IG.png?raw=true)

* Our Web Apps now must be associated with the route table we've created to allow internet access to our VPC.                                                      
* We can achieve this by choosing route tables, subnet associations and edit subnet associations.
* I will choose Web1 and Web2 which has public subnets.

![](https://github.com/CTheTechGuru/AWS-Networking-3-Tier-Architecture/blob/main/images/Subnet%20Associations.png?raw=true)


## 4. Create EC2 Instances 

###  Create EC2 Instance

* Open the EC2 dashboard choose launch instance.
  
![](https://github.com/CTheTechGuru/AWS-Networking-3-Tier-Architecture/blob/main/images/Launch%20an%20EC2%20Instance.png?raw=true)

* There are a few settings we will use.
1. For Network we will choose out VPC
2. For Subnet we will choose our corresponding subnet for the EC2 we are creating.
3. Auto Assign IP for our Web tier applications only.(Web1/Web2)
4. Default for remaining settings.
5. Use the default security group ( We will create a SG for our WEB1 App to enable SSH)
6. Use existing key pair if you have or create new if you do not have one. _.PPK_
7. Add Tags to identify instances.   

![](https://github.com/CTheTechGuru/AWS-Networking-3-Tier-Architecture/blob/main/images/Instance%20settings.png?raw=true)

 

 
## 5. Create Security Group with SSH Inbound & Outbound Rules  
* Go to the VPC dashboard and scroll until you see security groups.
* Choose Create Security Groups to correspond with the image below
* Inbound Rules - HTTP, HTTPS, SSH Custom 0.0.0.0/0) Outbound All Traffic 0.0.0.0/0

![](https://github.com/CTheTechGuru/AWS-Networking-3-Tier-Architecture/blob/main/images/SSH%20SG%20Create.PNG?raw=true)

* Now go to the EC2 dashboard and highlight the WEB1 EC2 Instance, click the actions drop down, then security, security groups.

![](https://github.com/CTheTechGuru/AWS-Networking-3-Tier-Architecture/blob/main/images/SSH%20Security%20Group.PNG?raw=true) 

* Remove the default and add the SSH SG one created. 





## 6.  Create Bastion Host & Setup Security Groups

1. Why we need a bastion host? We use the bastion host to allow us to connect to our private subnets which do not have internet access.

* This is a security practice that ensures Databases and other critical infrastructure is secured, Upholding confidentiality, integrity and availability. 

* First will create a EC2 instance that will be our bastion host. This EC2 will be in the same subnet as our Web server. We will auto assign a public IP as well. 
* These settings should be very similar to the WEB1 EC2 Instance.

![](https://github.com/CTheTechGuru/AWS-Networking-3-Tier-Architecture/blob/main/images/Bastion%20Host.PNG?raw=true)

* Next create a SG for the bastion host. We Will Only Allow SSH from 0.0.0.0/0

![](https://github.com/CTheTechGuru/AWS-Networking-3-Tier-Architecture/blob/main/images/SG%20BH.PNG)

* Now from the EC2 dashboard and attach the SG you just made to the bastion host instance.

![](https://github.com/CTheTechGuru/AWS-Networking-3-Tier-Architecture/blob/main/images/BH%20Instance%20SG.PNG?raw=true)

![](https://github.com/CTheTechGuru/AWS-Networking-3-Tier-Architecture/blob/main/images/Change%20SG.PNG?raw=true)

## 7. Create Security Group SSH Access DB1
 
 * For us to access the DB, we need to setup an AllowSSH Security Group for our Database to allow a connection from our Bastion host. 
 * Because our DB is in a private subnet, we have to access through SSH which keeps our data secure within our well architected framework.

![](https://github.com/CTheTechGuru/AWS-Networking-3-Tier-Architecture/blob/main/images/BH%20Instance%20SG.PNG?raw=true)

![](https://github.com/CTheTechGuru/AWS-Networking-3-Tier-Architecture/blob/main/images/Change%20SG.PNG?raw=true)

## 8. Configure Putty and Pageant to access the DB from out Bastion Host

* Open Putty and enter the public ip of the bastion host in the host name field.
* Choose SSH port 22 for connection type
* On the left column under connection choose SSH/Auth.
* Find your .ppk key file from earlier during the creation of your account.
* Go back under "Session" Saved Sessions name enter AWS Bastion Host and hit save.
* Your putty session should resemble below.

![](https://github.com/CTheTechGuru/AWS-Networking-3-Tier-Architecture/blob/main/images/Bastion%20Host%20Putty%20Login.PNG?raw=true)
![](https://github.com/CTheTechGuru/AWS-Networking-3-Tier-Architecture/blob/main/images/PuttyAgent.PNG?raw=true)

* To pass our key into the ssh session we are attempting to establish with the DB1 Server. We will need to use Pageant.
* Open Pageant, select add key. Choose the .ppk key file.


  
 
## 9. Connect To DB1 from our Bastion Host

* Open Putty, choose the AWS Bastion Host profile saved in the previous step, select load.
 
![](https://github.com/CTheTechGuru/AWS-Networking-3-Tier-Architecture/blob/main/images/Logged%20into%20bastion%20host.PNG?raw=true)

* We are now logged into the bastion host.
* Now to log into the DB, enter ssh (Private-Ip-Of-BH)

![](https://github.com/CTheTechGuru/AWS-Networking-3-Tier-Architecture/blob/main/images/DB%20Connection.PNG)

* We've established a connection to the DB. 




 
## 10. Implement NAT Gateway to provide Internet Access to the EC2 Instances in Private Subnets. 

* In the VPC dashboard choose NAT Gateway and Create.
* Attach the public subnet for Web1.
* Create a New Elastic IP and Create NAT Gateway.
![](https://github.com/CTheTechGuru/AWS-Networking-3-Tier-Architecture/blob/main/images/NAT%20GW.png?raw=true)

* We want for our DB to have access to download patches to stay up to date which why we need internet access.
* We need to create a new route table to allow the NAT gateway to have a route to the internet. 
![](https://github.com/CTheTechGuru/AWS-Networking-3-Tier-Architecture/blob/main/images/NAT%20GW2.PNG)

* Next change the route table of DB1 to the NAT Gateway route table.
 ![](https://github.com/CTheTechGuru/AWS-Networking-3-Tier-Architecture/blob/main/images/NAT%20Route%20Table%20Association.PNG)

* To test we will run a ping.

![](https://github.com/CTheTechGuru/AWS-Networking-3-Tier-Architecture/blob/main/images/DB%20Access%20Through%20NAT%20Gateway.PNG?raw=true)

* This allows the DB tier to access the internet through the nat gateway and update patches when needed without a public ip.

<h1 align="center">Summary</h3>

* We created a VPC and created 6 subnets, route table, and internet gateway for internet access.
* Provisioned EC2 instances for Web, App, and DB Tiers. 
* Configured security group inbound outbound rules to allow SSH. 
* Setup a Bastion Host Instance with SSH only access in a public subnet.
* Configured DB instance security group to allow inbound SSH from bastion host. 
* Configured Putty to allow SSH connections to our instances.
* Added Key to Peagent for session use when connecting to DB instance from bastion host. 
* Connected to DB Instance from Bastion Host.
* Configured NAT Gateway to allow internet access to private subnets.

This concludes the project, of course that are many architectural designs. 
My emphasis on this project was more focused on the networking components in 
AWS and how to maintain a secure environment within the console. 





<!-- CONTACT -->
## Contact

Cordelra Lowman - Cordelra_Lowman@yahoo.com

<h3 align="left">Follow me on Linkedin:</h3>
<p align="left">
<a href="https://linkedin.com/in/cordelra lowman" target="blank"><img align="center" src="https://raw.githubusercontent.com/rahuldkjain/github-profile-readme-generator/master/src/images/icons/Social/linked-in-alt.svg" alt="cordelra lowman" height="30" width="40" /></a>
</p>






