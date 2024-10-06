**<u>Virtual Private Cloud (VPC) Documentation</u>**

**VPC Requirements**

1.  CIDR Block: 10.0.0.0/16

2.  Region: us-west-2

3.  Availability Zones: us-west-2a, us-west-2b, us-west-2c

4.  Subnets: 15 Subnets (One per availability Zone)

**STEP 1:**

We shall first create a VPC for our project and assign an internet
gateway to it

- Login to your AWS management console and search VPC

<img src="./media/image1.jpeg"
style="width:6.26806in;height:3.05972in" />

<img src="./media/image2.jpeg"
style="width:6.26806in;height:3.05972in" />

<img src="./media/image3.jpeg"
style="width:5.56944in;height:2.7187in" />

<img src="./media/image4.jpeg" style="width:5.56962in;height:2.71878in"
alt="A screenshot of a computer Description automatically generated" />

- Now we create an Internet Gateway

<img src="./media/image5.jpeg"
style="width:5.72152in;height:2.81322in" />

<img src="./media/image6.jpeg"
style="width:5.29114in;height:2.6016in" />

<img src="./media/image7.jpeg"
style="width:5.53165in;height:4.15517in" />

<img src="./media/image8.jpeg"
style="width:6.26806in;height:2.20694in" />

We will follow the following subnet naming schemes:

> *EnvName-AppType-RouteType-AZ*

For example,

*Prod-Web-Public-2a*

Let's create some subnet

- Public Subnets

| Subnet Name        | Availability Zone | CIDR Block   | Type   |
|--------------------|-------------------|--------------|--------|
| Prod-Web-Public-2a | us-west-2a        | 10.0.0.0/28  | Public |
| Prod-Web-Public-2b | us-west-2b        | 10.0.0.16/28 | Public |
| Prod-Web-Public-2c | us-west-2c        | 10.0.0.32/28 | Public |

**Using the above table data, we will create the public subnet**

<img src="./media/image9.jpeg" style="width:6.26806in;height:1.75in" />

<img src="./media/image10.jpeg"
style="width:6.04167in;height:8.875in" />

<img src="./media/image11.jpeg"
style="width:5.09437in;height:4.63268in" />

<img src="./media/image12.jpeg"
style="width:5.18232in;height:4.71266in" />

<img src="./media/image13.jpeg"
style="width:6.26806in;height:1.38194in" />

During the process of creating the subnets, the following error was
encountered. To overcome this, the three subnets were created
separately.

<img src="./media/image14.jpeg"
style="width:6.26806in;height:3.14931in" />

**Using the above steps, create the other subnets with the data below**

- Application Subnets

| Subnet Name         | Availability Zone | CIDR Block   | Type    |
|---------------------|-------------------|--------------|---------|
| Prod-App-Private-2a | us-west-2a        | 10.0.0.48/28 | Private |
| Prod-App-Private-2b | us-west-2b        | 10.0.0.64/28 | Private |
| Prod-App-Private-2c | us-west-2c        | 10.0.0.80/28 | Private |

- Database Subnets

| Subnet Name        | Availability Zone | CIDR Block    | Type    |
|--------------------|-------------------|---------------|---------|
| Prod-DB-Private-2a | us-west-2a        | 10.0.0.96/28  | Private |
| Prod-DB-Private-2b | us-west-2b        | 10.0.0.112/28 | Private |
| Prod-DB-Private-2c | us-west-2c        | 10.0.0.128/28 | Private |

- Management Subnets

| Subnet Name          | Availability Zone | CIDR Block    | Type    |
|----------------------|-------------------|---------------|---------|
| Prod-Mgmt-Private-2a | us-west-2a        | 10.0.0.144/28 | Private |
| Prod-Mgmt-Private-2b | us-west-2b        | 10.0.0.160/28 | Private |
| Prod-Mgmt-Private-2c | us-west-2c        | 10.0.0.176/28 | Private |

- Platform Subnets

| Subnet Name              | Availability Zone | CIDR Block    | Type    |
|--------------------------|-------------------|---------------|---------|
| Prod-Platform-Private-2a | us-west-2a        | 10.0.0.192/28 | Private |
| Prod-Platform-Private-2b | us-west-2b        | 10.0.0.208/28 | Private |
| Prod-Platform-Private-2c | us-west-2c        | 10.0.0.224/28 | Private |

**This is how it should look like after you create all the subnets**

<img src="./media/image15.jpeg"
style="width:6.26806in;height:2.2875in" />

**Route Table Design**

For each subnet group, we will create a custom route table and assign
rules required for the specific subnets.

For example, all three public subnets will share the same public-subnet
route table.

| Subnet     | Destination CIDR | Target           |
|------------|------------------|------------------|
| Public     | 0.0.0.0/0        | Internet Gateway |
| App        | 0.0.0.0/0        | Nat Gateway      |
| DB         | 0.0.0.0/0        | Nat Gateway      |
| Management | 0.0.0.0/0        | Nat Gateway      |
| Platform   | 0.0.0.0/0        | Nat Gateway      |

Using the table above you will create 5 route tables

- Let's create public route table

<img src="./media/image16.jpeg"
style="width:6.26806in;height:2.2875in" />

<img src="./media/image17.jpeg"
style="width:6.26806in;height:5.40417in" />

<img src="./media/image18.jpeg"
style="width:6.26806in;height:2.37639in" />

<img src="./media/image19.jpeg"
style="width:6.26806in;height:2.37639in" />

- We add 3 public subnets (prod-web-public a,b,c)

<img src="./media/image20.jpeg"
style="width:6.26806in;height:3.09722in" />

<img src="./media/image21.jpeg"
style="width:6.26806in;height:2.19306in" />

**Using the table and steps above create the other route tables and
their subnet association**

<img src="./media/image22.jpeg"
style="width:6.26806in;height:2.35347in" />

**NAT Gateway**

A NAT gateway is a Network Address Translation (NAT) service. You can
use a NAT gateway so that instances in a private subnet can connect to
services outside your VPC but external services cannot initiate a
connection with those instances.

We need to create a NAT gateway and attach it to all our route tables
created earlier

<img src="./media/image23.jpeg"
style="width:6.26806in;height:2.35347in" />

- select a subnet and allocate an elastic ip

<img src="./media/image24.jpeg"
style="width:5.62025in;height:6.25849in" />

**Now we will add the NAT gateway to our route tables one by one:**

First of all, add route to each route table, we are adding to platorm
route table now. For Destination select 0.0.0.0/0, and for the Target
select NAT gateway. Select the NAT gateway created earlier and save
changes

<img src="./media/image25.jpeg"
style="width:6.26806in;height:3.11875in" />

<img src="./media/image26.jpeg"
style="width:6.26806in;height:3.11875in" />

<img src="./media/image27.jpeg" style="width:6.26806in;height:1.52014in"
alt="A screenshot of a computer Description automatically generated" />

- Notice the status of the NAT gateway it's active

<img src="./media/image28.jpeg"
style="width:6.26806in;height:2.13403in" />

**Do the above steps for the other route tables ie for the DB, APP,
MANAGEMENT ROUTE TABLES**

If you noticed we did not do anything to our public subnet we want to
route traffic uing a service called internet gateway so let's attach it
to the public route table

<img src="./media/image29.jpeg"
style="width:6.26806in;height:2.85278in" />

<img src="./media/image30.jpeg"
style="width:6.26806in;height:1.50694in" />

<img src="./media/image31.jpeg"
style="width:6.26806in;height:2.39444in" />

**AWS VPC Topology**

The following diagram shows the high-level VPC topology for our design.

Note: Both the internet Gateway (IGW) and NAT gateway(NAT-GW) gets
deployed in the public subnet.

To check our VPC topology:

<img src="./media/image32.jpeg"
style="width:6.26806in;height:3.07917in" />

<img src="./media/image33.jpeg"
style="width:6.26806in;height:3.65417in" />

**Network ACLs**

Network access control list (NACL) is the native VPC functionality to
control the inbound and outbound traffic at the subnet level.

In our architecture, the connection to the DB subnet should be allowed
only from the App subnet and management subnet. The public subnet should
not have direct access to the DB subnet.

The following are the tables for inbound and outbound rules for the DB
NACL.

**DB NACL (Inbound Rules)**

| Rule Number | Type        | Protocol | Port Range | Source IP     | Allow/Deny |
|-------------|-------------|----------|------------|---------------|------------|
| 100         | Custom TCP  | TCP      | 3306       | 10.0.0.96/28  | Allow      |
| 110         | Custom TCP  | TCP      | 3306       | 10.0.0.112/28 | Allow      |
| 120         | Custom TCP  | TCP      | 3306       | 10.0.0.128/28 | Allow      |
| \*          | All Traffic | All      | All        | 0.0.0.0/0     | Deny       |

**DB NACL (Outbound Rules)**

| Rule Number | Type        | Protocol | Port Range | Destination IP | Allow/Deny |
|-------------|-------------|----------|------------|----------------|------------|
| 100         | Custom TCP  | TCP      | 3306       | 10.0.0.192/28  | Allow      |
| 110         | Custom TCP  | TCP      | 3306       | 10.0.0.208/28  | Allow      |
| 120         | Custom TCP  | TCP      | 3306       | 10.0.0.224/28  | Allow      |
| \*          | All Traffic | All      | All        | 0.0.0.0/0      | Deny       |

The above table serves as a guide to how your implemetation would look
like: Here is a step by step on a Network ACLS:

<img src="./media/image34.jpeg"
style="width:6.26806in;height:1.88958in" />

<img src="./media/image35.jpeg"
style="width:6.26806in;height:5.04514in" />

<img src="./media/image36.jpeg"
style="width:6.26806in;height:3.10764in" />

<img src="./media/image37.jpeg"
style="width:6.26806in;height:1.57153in" />

<img src="./media/image38.jpeg"
style="width:6.26806in;height:3.15625in" />

<img src="./media/image39.jpeg"
style="width:6.26806in;height:1.51458in" />

<img src="./media/image40.jpeg"
style="width:6.26806in;height:3.15625in" />
