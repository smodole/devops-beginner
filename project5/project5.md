**DOCUMENTATION**

1.  Refer to Project 1 on how to spin up an Ubuntu Server

2.  Create four instances of the server and rename them for Consul
    Server (Consul), Load Balancer (LB), First Backend (BE1), and Second
    Backend (BE2)

<img src="./media/image1.jpeg"
style="width:5.91981in;height:1.54455in" />

3.  Open the following ports in the Security Group for the Consul Server

| **S/N** | **Port Name** | **Protocol** | **Default Port** |
|---------|---------------|--------------|------------------|
| 1       | DNS           | TCP and UDP  | 8600             |
| 2       | HTTP API      | TCP          | 8500             |
| 3       | HTTPS API     | TCP          | 8501             |
| 4       | gRPC          | TCP          | 8502             |
| 5       | gRPC TLS      | TCP          | 8503             |
| 6       | Server RPC    | TCP          | 8300             |
| 7       | LAN Serf      | TCP and UDP  | 8301             |
| 8       | WAN Serf      | TCP and UDP  | 8302             |

4.  Select the Consul instance, click on the Security tab and copy the
    Security Group ID (e.g. sg-0d7ecfda5e1aa3dce)

<img src="./media/image2.jpeg"
style="width:6.26806in;height:3.27083in" />

5.  Click on Edit Inbound Rule

<img src="./media/image3.jpeg"
style="width:6.26806in;height:1.70278in" />

6.  Click on Add Rule

<img src="./media/image4.jpeg"
style="width:6.26806in;height:2.38958in" />

7.  Enter port range for TCP (8600), and choose the appropriate CIDR
    block

<img src="./media/image5.jpeg" style="width:6.26806in;height:2.38958in"
alt="A screenshot of a computer Description automatically generated" />

8.  Click on Add Rule to specify the port range for the UDP (8600)
    protocol, and choose the appropriate CIDR block

<img src="./media/image6.jpeg"
style="width:6.26806in;height:2.87778in" />

9.  Repeat the process until all the ports above are opened

<img src="./media/image7.jpeg"
style="width:6.26806in;height:3.06528in" />

10. Click on **Save rules** to apply the updated security group settings

<img src="./media/image8.jpeg" style="width:6.26806in;height:1.99583in"
alt="A screenshot of a computer Description automatically generated" />

11. Set up the Consul Server

12. SSH into the consul server and run **sudo apt update** to refresh
    the package cache

<img src="./media/image9.jpeg"
style="width:4.90099in;height:2.23059in" />

<img src="./media/image10.jpeg"
style="width:6.26806in;height:2.85278in" />

13. Visit the
    consul [**downloads**](https://developer.hashicorp.com/consul/install) page
    to **copy** the installation command.

> <img src="./media/image11.jpeg"
> style="width:6.26806in;height:3.37778in" />

14. Execute the copied command:

***wget -O- https://apt.releases.hashicorp.com/gpg \| sudo gpg --dearmor
-o /usr/share/keyrings/hashicorp-archive-keyring.gpg***

***echo "deb
\[signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg\]
https://apt.releases.hashicorp.com \$(lsb_release -cs) main" \| sudo tee
/etc/apt/sources.list.d/hashicorp.list***

***sudo apt update && sudo apt install consul***

<img src="./media/image12.jpeg"
style="width:6.26806in;height:0.43472in" />

15. Confirm Consul installation by checking its version with the
    ***consul –version*** command

<img src="./media/image13.jpeg"
style="width:6.26806in;height:0.86319in" />

16. All the Consul server configurations are located in
    the **/etc/consul.d** folder. To configure the Consul server, start
    by backing up the default configuration file **consul.hcl** by
    renaming it to **consul.hcl.back**, using the following
    command: **sudo mv /etc/consul.d/consul.hcl
    /etc/consul.d/consul.hcl.back**

<img src="./media/image14.jpeg"
style="width:6.26806in;height:0.23542in" />

17. G enerate an **encrypted key** using the **consul keygen** command
    +urdi+sIty0FESDljEljpPKiie/yKTn9AOcEYCztDRs=

<img src="./media/image15.jpeg"
style="width:6.26806in;height:0.52361in" />

18. Create a new file named **consul.hcl** in
    the **/etc/consul.d** directory, using the following command: **sudo
    vi /etc/consul.d/consul.hcl**

<img src="./media/image16.jpeg"
style="width:6.26806in;height:0.52361in" />

19. Add the following content to the **consul.hcl** file,
    replacing **\<YOUR_ENCRYPTED_KEY\>** with the encrypted key you
    generated:

<img src="./media/image17.jpeg" style="width:6.26806in;height:1.18681in"
alt="A screen shot of a computer Description automatically generated" />

20. R un the following command to start the Consul server in the
    background: **sudo nohup consul agent -dev -config-dir
    /etc/consul.d/ &**

<img src="./media/image18.jpeg"
style="width:6.26806in;height:0.63889in" />

21. You can check the status of the Consul server with the following
    command: **consul members**

<img src="./media/image19.jpeg"
style="width:6.26806in;height:1.53958in" />

22. The following error was encountered while checking the status of the
    consul server

<img src="./media/image20.jpeg"
style="width:6.26806in;height:0.95556in" />

23. To solve the above error, I regenerated the consul key, add it to
    the configuration file and then restarted the consul server again.

24. When you visit **\<EC2 Consul Server IP\>:8500**, you should be able
    to access the Consul dashboard.

<img src="./media/image21.jpeg"
style="width:6.26806in;height:3.43958in" />

**SETTING UP BACKEND SERVERS**

1.  To set up the two backend servers, install the Nginx and Consul
    Agents on each of the servers by first SSH into the servers and run
    the following command: **sudo apt-get update -y**

<img src="./media/image22.jpeg"
style="width:6.26806in;height:1.34167in" />

2.  Install Nginx on both instances by running the following
    command: **sudo apt install nginx -y**

<img src="./media/image23.jpeg"
style="width:6.26806in;height:4.77639in" />

3.  Navigate to the default HTML directory and modify the index.html
    file on both servers to differentiate them

<img src="./media/image24.jpeg"
style="width:6.26806in;height:0.60486in" />

<img src="./media/image25.jpeg"
style="width:5.78218in;height:1.47085in" />

<img src="./media/image26.jpeg" style="width:5.84158in;height:1.48596in"
alt="A screen shot of a computer Description automatically generated" />

4.  Install Consul as an agent on the servers. Run the following
    commands to install Consul:

wget -O- https://apt.releases.hashicorp.com/gpg \| gpg --dearmor \|
***sudo tee /usr/share/keyrings/hashicorp-archive-keyring.gpg***

***echo "deb
\[signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg\]
https://apt.releases.hashicorp.com \$(lsb_release -cs) main" \| sudo tee
/etc/apt/sources.list.d/hashicorp.list***

***sudo apt update && sudo apt install consul***

<img src="./media/image27.jpeg"
style="width:5.70297in;height:1.4507in" />

5.  Verify that Consul is installed properly by running the following
    command: **consul –version**

<img src="./media/image28.jpeg"
style="width:5.34653in;height:1.00818in" />

6.  Replace the default Consul configuration file **config.hcl** located
    in **/etc/consul.d** with your custom **consul.hcl** file by
    renaming the default file and create a new one by running the
    following commands:

**sudo mv /etc/consul.d/consul.hcl /etc/consul.d/consul.hcl.back**

**sudo vi /etc/consul.d/consul.hcl**

<img src="./media/image29.jpeg"
style="width:6.26806in;height:1.18194in" />

<img src="./media/image30.jpeg"
style="width:6.26806in;height:1.18194in" />

7.  Create a **backend.hcl** configuration file in
    the **/etc/consul.d** directory to register the Nginx service and
    its health check URLs with the Consul server using the following
    command: **sudo vi /etc/consul.d/backend.hcl**

<img src="./media/image31.jpeg"
style="width:6.26806in;height:1.18194in" />

8.  Start the Consul agent with the following command: **sudo nohup
    consul agent -config-dir /etc/consul.d/ &**

9.  Verify if everything is working correctly by visiting the Consul UI:

> **54.160.49.46:8500**

<img src="./media/image32.jpeg"
style="width:6.26806in;height:1.89236in" />

<img src="./media/image33.jpeg"
style="width:6.26806in;height:1.89236in" />

<img src="./media/image34.jpeg"
style="width:6.26806in;height:3.44931in" />

**SETTING UP LOAD BALANCER**

1.  Log in to the load-balancer server. Update the package information
    and install unzip with the following commands:

**sudo apt-get update -y**

**sudo apt-get install unzip -y**

<img src="./media/image35.jpeg"
style="width:6.26806in;height:0.87569in" />

<img src="./media/image36.jpeg"
style="width:6.26806in;height:4.50972in" />

2.  Install Nginx using the following command: **sudo apt install nginx
    -y**

<img src="./media/image37.jpeg"
style="width:6.26806in;height:0.89653in" />

3.  Download the consul-template binary using the following command:

**sudo curl -L
https://releases.hashicorp.com/consul-template/0.30.0/consul-template_0.30.0_linux_amd64.zip
-o /opt/consul-template.zip**

**sudo unzip /opt/consul-template.zip -d /usr/local/bin/**

<img src="./media/image38.jpeg"
style="width:6.26806in;height:1.33542in" />

4.  To verify the installation of consul-template, check its version
    with the following command: **consul-template –version**

<img src="./media/image39.jpeg"
style="width:6.26806in;height:0.82083in" />

5.  Create and edit a file named **load-balancer.conf.ctmpl** in
    the **/etc/nginx/conf.d** directory, using the following
    command: **sudo vi /etc/nginx/conf.d/load-balancer.conf.ctmpl**

<img src="./media/image40.jpeg" style="width:6.26806in;height:1.57917in"
alt="A black screen with white text Description automatically generated" />

6.  Create and edit the file: **sudo vi
    /etc/nginx/conf.d/consul-template.hcl**

<img src="./media/image41.jpeg"
style="width:6.26806in;height:1.80208in" />

7.  Delete the default server configuration to disable it by running the
    following command: **sudo rm /etc/nginx/sites-enabled/default**

8.  R estart Nginx to apply the changes by running the following
    command: **sudo systemctl restart nginx**

<img src="./media/image42.jpeg" style="width:6.26806in;height:0.89028in"
alt="A screen shot of a computer Description automatically generated" />

9.  Start the Consul Template agent using the following command. It
    continuously monitors Consul for changes

**sudo nohup consul-template
-config=/etc/nginx/conf.d/consul-template.hcl &**

<img src="./media/image43.jpeg" style="width:6.26806in;height:0.89028in"
alt="A screenshot of a computer Description automatically generated" />

10. Upon completion, a load-balancer.conf file will be created with
    backend server information populated from the Consul service
    registry

<img src="./media/image44.jpeg"
style="width:6.26806in;height:2.00347in" />

11. If you access the load balancer IP in your web browser, it will
    display the custom HTML content from one of the backend servers.
    When you refresh the page, the load balancer will route your request
    to the other backend server, displaying its custom HTML content

<img src="./media/image45.jpeg"
style="width:6.26806in;height:1.69792in" />

> <img src="./media/image46.jpeg"
> style="width:6.26806in;height:1.69792in" />

**SERVICE DISCOVERY TEST**

1.  Test the configuration by observing what happens when you stop one
    of your backend servers

<img src="./media/image47.jpeg"
style="width:6.26806in;height:0.68264in" />

<img src="./media/image48.jpeg"
style="width:6.26806in;height:3.39792in" />

<img src="./media/image49.jpeg"
style="width:6.26806in;height:1.86667in" />

2.  As a result, the load balancer will only direct traffic to the
    remaining healthy backend servers. This ensures that your
    application continues to run smoothly without any disruption to
    users, demonstrating the effectiveness of your service discovery and
    health check configuration with Consul and Nginx.

<img src="./media/image50.jpeg"
style="width:6.26806in;height:3.60069in" />
