Connect your instance via ssh:

<img src="./media/image1.jpeg"
style="width:6.26806in;height:2.49792in" />

Clone the project repository to your instance:

***git clone
https://github.com/TobiOlajumoke/prometheus-observability-stack***

Here is the project structure and config files:

<img src="./media/image2.jpeg"
style="width:6.26806in;height:2.49792in" />

Spin up an ec2 instance and attach the following IAM roles to it:

<img src="./media/image3.jpeg"
style="width:4.44304in;height:2.16688in" />

<img src="./media/image4.jpeg" style="width:6.26806in;height:3.05694in"
alt="A screenshot of a computer Description automatically generated" />

<img src="./media/image5.jpeg"
style="width:6.26806in;height:3.14514in" />

<img src="./media/image6.jpeg" style="width:5.9788in;height:3in" />

<img src="./media/image7.jpeg"
style="width:5.62562in;height:2.82278in" />

<img src="./media/image8.jpeg"
style="width:5.72152in;height:2.8709in" />

**Create an EC2 instance, Ubuntu 22.04**

<img src="./media/image9.jpeg"
style="width:5.6076in;height:2.81374in" />

<img src="./media/image10.jpeg"
style="width:6.26806in;height:1.41667in" />

**Connect to your instance via ssh**

- ssh into the instance

*ssh -i "project10.pem"
ubuntu@ec2-18-213-231-230.compute-1.amazonaws.com*

<img src="./media/image11.jpeg"
style="width:6.26806in;height:2.49444in" />

- Install terraform using the following command:

***sudo snap install terraform –classic***

<img src="./media/image12.jpeg" style="width:6.26806in;height:1.29583in"
alt="A black rectangular object with a black background Description automatically generated" />

**Provision Server Using Terraform**

- Change directory using the following:

***cd /home/ubuntu/prometheus-observability-stack/terraform-aws/vars***

<img src="./media/image13.jpeg"
style="width:6.26806in;height:2.49444in" />

- If you are using us-west-2, you can continue with the same AMI ID else
  change the AMI ID using: ***vi ec2.tfvars***

<img src="./media/image14.jpeg"
style="width:6.26806in;height:2.49444in" />

- fill it with the correct variable like this:

*VPC ID: vpc-0b211b2627066fbaf*

*SUBNET ID: subnet-097d92d163d0a7ce7*

*AMI ID: ami-0ea3c35c5c3284d82*

<img src="./media/image15.jpeg" style="width:6.26806in;height:2.49444in"
alt="A screenshot of a computer Description automatically generated" />

- Now we can provision the AWS EC2 & Security group using Terraform.

> *cd ../prometheus-stack*
>
> *terraform fmt*
>
> *terraform init*
>
> *terraform validate*

<img src="./media/image16.jpeg" style="width:6.26806in;height:2.67917in"
alt="A screenshot of a computer Description automatically generated" />

- Execute the plan and apply the changes.

terraform plan --var-file=../vars/ec2.tfvars

<img src="./media/image17.jpeg"
style="width:5.36484in;height:2.2931in" />

terraform apply --var-file=../vars/ec2.tfvars

<img src="./media/image18.jpeg" style="width:5.46552in;height:3.00464in"
alt="A computer screen with white text Description automatically generated" />

Connecting to our new instance

<img src="./media/image19.jpeg" style="width:5.30417in;height:2.89655in"
alt="A screenshot of a computer Description automatically generated" />

<img src="./media/image20.jpeg" style="width:6.26806in;height:2.85486in"
alt="A screenshot of a computer Description automatically generated" />

Run the following command:

ssh -i "terraf.pem"
ubuntu@ec2-3-144-211-142.us-east-2.compute.amazonaws.com

<img src="./media/image21.jpeg" style="width:6.26806in;height:3.35417in"
alt="A screenshot of a computer screen Description automatically generated" />

We will check the cloud-init logs to see if the user data script has run
successfully.

> tail /var/log/cloud-init-output.log

<img src="./media/image22.jpeg" style="width:6.26806in;height:1.2625in"
alt="A screenshot of a computer Description automatically generated" />

Let’s verify the docker and docker-compose versions again.

sudo docker version

<img src="./media/image23.jpeg" style="width:6.26806in;height:1.04583in"
alt="A black background with a black border Description automatically generated" />

**Deploy Prometheus Stack Using Docker Compose**

- First, clone the project code repository to the server.

git clone
https://github.com/TobiOlajumoke/prometheus-observability-stack

<img src="./media/image24.jpeg" style="width:6.26806in;height:1.04583in"
alt="A screen shot of a computer Description automatically generated" />

- cd prometheus-observability-stack

- Execute the following make command to update server IP in prometheus
  config file. Because we are running the node exporter on the same
  server to fetch the server metrics. We also update the alert manager
  endpoint to the servers public IP address.

> make all

<img src="./media/image25.jpeg" style="width:6.26806in;height:2.76528in"
alt="A black screen with white text Description automatically generated" />

- **Bring up the stack using Docker Compose. It will deploy Prometheus,
  Alert manager, Node exporter and Grafana**

> sudo docker-compose up -d

<img src="./media/image26.jpeg" style="width:6.26806in;height:3.21111in"
alt="A screenshot of a computer Description automatically generated" />

- Now, with your servers IP address you can access all the apps on
  different ports

1.  Prometheus: [http://your-public-ip-address:9090](http://your-public-ip-address:9090/)

2.  Alert
    Manager: [http://your-public-ip-address:9093](http://your-public-ip-address:9093/)

3.  Grafana: [http://your-public-ip-address:3000](http://your-public-ip-address:3000/)

4.  Now the stack deployment is done. The rest of the configuration and
    testing will be done the using the GUI.

<img src="./media/image27.jpeg" style="width:6.26806in;height:2.44792in"
alt="A screenshot of a computer Description automatically generated" />

<img src="./media/image28.jpeg" style="width:6.26806in;height:2.44792in"
alt="A screenshot of a computer Description automatically generated" />

<img src="./media/image29.jpeg" style="width:6.26806in;height:3.28889in"
alt="A screenshot of a computer Description automatically generated" />

<img src="./media/image30.jpeg" style="width:6.26806in;height:3.28889in"
alt="A screenshot of a computer program Description automatically generated" />

<img src="./media/image31.jpeg" style="width:6.26806in;height:1.62917in"
alt="A screenshot of a computer Description automatically generated" />

- **validating prometheus rules and targets**

Now lets execute a promQL statement to view node_cpu_seconds_total
metrics scrapped from the node exporter. Click on “Graph” menu

<img src="./media/image32.jpeg" style="width:6.26806in;height:2.00347in"
alt="A screenshot of a computer Description automatically generated" />

You should be able to data in graph as shown below.

<img src="./media/image33.jpeg" style="width:6.26806in;height:2.70069in"
alt="A screenshot of a computer Description automatically generated" />

<img src="./media/image34.jpeg" style="width:4.86207in;height:2.50699in"
alt="A screenshot of a computer Description automatically generated" />

**Configure Grafana Dashboards**

Grafana can be accessed
at: [http://your-ip-address:3000](http://your-ip-address:3000/)

<img src="./media/image35.jpeg" style="width:4.65517in;height:2.40031in"
alt="A screenshot of a computer Description automatically generated" />

Use admin as username and password to login to Grafana. You can update
the password in the next window if required.

<img src="./media/image36.jpeg" style="width:4.86181in;height:2.50685in"
alt="A screenshot of a computer Description automatically generated" />

Now we need to add prometheus URL as the data source from Connections→
Add new connection→ Prometheus → Add new data source.

<img src="./media/image37.jpeg" style="width:6.26806in;height:1.92986in"
alt="A screenshot of a computer Description automatically generated" />

<img src="./media/image38.jpeg" style="width:6.26806in;height:1.92986in"
alt="A screenshot of a computer Description automatically generated" />

<img src="./media/image39.jpeg" style="width:6.26806in;height:3.30139in"
alt="A screenshot of a computer Description automatically generated" />

<img src="./media/image40.jpeg" style="width:6.26806in;height:3.30139in"
alt="A screenshot of a computer Description automatically generated" />

<img src="./media/image41.jpeg" style="width:6.26806in;height:3.30139in"
alt="A screenshot of a computer Description automatically generated" />

**Configure Node Exporter Dashboard**

Grafana has many node exporter pre-built templates that will give us a
ready to use dashboard for the key node exporter metrics.

To import a dashboard, go to Dashboards –\> Create Dashboard –\> Import
Dashboard –\> Type 10180 and click load –\> Select Prometheus Data
source –\> Import

<img src="./media/image42.jpeg" style="width:5.5in;height:2.89685in"
alt="A screenshot of a computer Description automatically generated" />

<img src="./media/image43.jpeg" style="width:5.72414in;height:3.01491in"
alt="A screenshot of a computer Description automatically generated" />

<img src="./media/image44.jpeg" style="width:5.60345in;height:2.95134in"
alt="A screenshot of a computer Description automatically generated" />

<img src="./media/image45.jpeg"
style="width:6.26806in;height:3.30139in" />

Once the dashbaord template is imported, you should be able to see all
the node exporter metrics as shown below.

<img src="./media/image46.jpeg"
style="width:6.26806in;height:3.30139in" />

**Simulate & Test Alert Manager Alerts**

You can access the Alertmanager dashbaord
on [http://your-ip-address:9093](http://your-ip-address:9093/)

Alert rules are already backed in to the prometheus configuration
through alertrules.yaml. If you go the alerts option in the prometheus
menu, you will be able to see the configured alerts as shown
below. [http://your-ip-address:9090](http://your-ip-address:9090/)

<img src="./media/image47.jpeg" style="width:6.26806in;height:2.05625in"
alt="A screenshot of a computer Description automatically generated" />

As you can see, all the alerts are in inactive stage. To test the
alerts, we need to simulate these alerts using few linux utilities.

You can also check the alert rules using the native promtool prometheus
CLI. We need to run promtool command from inside the prometheus
container as shown below. run the commands below in the prometheus
server

<img src="./media/image48.jpeg" style="width:6.26806in;height:1.89375in"
alt="A computer screen with white text Description automatically generated" />

**Test: High Storage & CPU Alert**

dd if=/dev/zero of=testfile_16GB bs=1M count=16384; openssl speed -multi
\$(nproc --all) &

Now we can check the Alert manager UI to confirm the fired alerts.

<img src="./media/image49.jpeg" style="width:6.26806in;height:1.92847in"
alt="A screenshot of a computer Description automatically generated" />

<img src="./media/image50.jpeg" style="width:6.26806in;height:1.92847in"
alt="A screenshot of a computer Description automatically generated" />

Now let’s rollback the changes and see the fired alerts has been
resolved.

rm testfile_16GB && kill \$(pgrep openssl)

<img src="./media/image51.jpeg" style="width:6.26806in;height:3.47014in"
alt="A screenshot of a computer Description automatically generated" />
