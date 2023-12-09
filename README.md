<h1 align="center">AWS Networking 3 Tier Architecture Project</h3>

![img]()





<!-- PROJECT Details-->
 # About The Project

In this project I will go through step by step how to architect a VPC environment consisting of a three tier architecture. 
This will consist of three layers.
1. Web Tier - Presentation also the UI of our users to access our applciation (ex: www.amazon.com) 
2. Application Tier - Backend aka logic layer. This layer is acts as the medium between to two Web and database layer.(user requests and data storing) 
3. Database Tier - Responsible for storing, managing, and retrieving information. ( DynamoDB, NoSQL, AmazonRDS, MySQL )

With the current state of technology and its fast paced evoloution, businesses strive to 
maintain resilency as well as fault tolerance to handle work loads beyond measure. 
This is where the AWS makes it seamless to utilize cloud services on demand, opening up 
a plethora of options for building secure, highly avaliable, reliabile, and performance efficient applications.  

 # Prerequisites


* Basic understanding of Computer Networks.
* Basic understanding of TCP/IP.
* Basic understanding of virtualization.
* Access to AWS (Free Tier or Paid)
* _Best Practice_ Create AWS user account with administrator access.

## Steps - Create VPC | Subnets | Route Table | IGW

### 1. Create VPC

* Open the VPC dashboard and on the left column choose Your VPC. 
* Give your VPC a name tag to identify. 
* Create a CIDR range for your VPC. Mines will be 10.16.0.0/16





![](https://github.com/CTheTechGuru/AWS-Networking-3-Tier-Architecture/blob/main/images/Create%20VPC.png?raw=true)

* When done click create to to build your vpc.  
* By default a route table, dhcp option set, nacl, and one security group.
* We will modify these later in the project to enhance security as we add more components. 



### 2. Create Subnets - We will create a subnet for 

* We will create our subnets now, this is where networking knowledge comes in handy.  
* Our Cidr is 10.16.0.0/16, we will create a subnet per environment (WEB, APP, DB).
* On the main VPC dashboard look on the leftr column and choose subnets and create subnets on the next page.
* This is an example of our first subnet.
  ![](https://github.com/CTheTechGuru/AWS-Networking-3-Tier-Architecture/blob/main/images/Subnet%20Settings.png?raw=true)
* For high avaliability our architecture will be spread across multiple avaliabiliy zones, create the remainder of subnets.
![](https://github.com/CTheTechGuru/AWS-Networking-3-Tier-Architecture/blob/main/images/Subnets.png?raw=true)
  

### 3. Create Internet Gateway and Route Table  

* In order to gain access to the internet we need to create a an internet gateway and routing table to access the internet.
* From ther VPC main dashboard on the left column choose internet gateways, name and click create.
* Now highlight the internet gateway we've created and choose the actions tab above and Attatch to VPC.
* Click the drop down and select attatch. 
![](https://github.com/CTheTechGuru/AWS-Networking-3-Tier-Architecture/blob/main/images/Attach%20IGW.png?raw=true)
* Now we will create a routing table and edit routes to add 0.0.0.0/0 as a destination, which will allow us to connect outside of our VPC to the internet.

* Now on the left column of the dashboard choose route tables. 
* Name the routing table and select the VPC you created. 
![]( https://github.com/CTheTechGuru/AWS-Networking-3-Tier-Architecture/blob/main/images/Route%20Table%20Internet.PNG?raw=true)

* Once the route table is created you will need to select edit the route to allow the IGW to have internet access.


Destination   | Target 
------------- | -------------
0.0.0.0/0     | IGW(Your Internet Gateway)


![](https://github.com/CTheTechGuru/AWS-Networking-3-Tier-Architecture/blob/main/images/Edit%20Routes.PNG)
![](https://github.com/CTheTechGuru/AWS-Networking-3-Tier-Architecture/blob/main/images/Route%20for%20IG.png?raw=true)

* Our Web Apps now must be associated with the route table we've created to allow internet access to our VPC.                                                      
* We can achieve this by choosing route tables, subnet associations and edit subnet asscoations.
* I will choose Web1 and Web2 which has public subnets.

![](https://github.com/CTheTechGuru/AWS-Networking-3-Tier-Architecture/blob/main/images/Subnet%20Associations.png?raw=true)


3.

4.

5.



1. 
    
2. 

3.

4.

5.

<!-- CONTACT -->
## Contact

Cordelra Lowman - Cordelra_Lowman@yahoo.com

Project Link: (https://github.com/CTheTechGuru/Docker-Projects/tree/main/SSH%20Server%20%7C%20Client%20Project%20))

<p align="right">(<a href="#readme-top">back to top</a>)</p>






