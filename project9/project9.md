**<u>DOCUMENTATION FOR DOCKER</u>**

**Dockerfile: Building Your First Image**

1.  **Provisioning An Instance**

- Spin up an Ubuntu 24.04 t2.micro this is where we'll be doing the
  project in

- SSH into the instance

<img src="./media/image1.jpeg"
style="width:5.63112in;height:1.85042in" />

2.  **Install and Start Docker**

- Update your package index using ***sudo apt update***

<img src="./media/image2.jpeg"
style="width:5.59573in;height:1.83879in" />

- Install Docker using ***sudo apt install docker.io -y***

<img src="./media/image3.jpeg"
style="width:5.52496in;height:1.81554in" />

- Start Docker using the following:

> ***sudo systemctl start docker***
>
> ***sudo systemctl enable docker***

- Check the Status of Docker using ***sudo systemctl status docker***

<img src="./media/image4.jpeg"
style="width:6.26806in;height:2.05972in" />

- Ctrl + C to exit the running docker

3.  **Clone the Docker Project**

> Once Docker is installed and set up, clone the project repository.

- Install Git (if not installed) using ***sudo apt install git -y***

<img src="./media/image5.jpeg"
style="width:6.04651in;height:1.98692in" />

- Clone the project repository using the following:

> ***git clone https://github.com/TobiOlajumoke/docker-flask***
>
> ***cd docker-flask***

<img src="./media/image6.jpeg"
style="width:5.54583in;height:1.8224in" />

- The docker file using ***cat dockerfile***

<img src="./media/image7.jpeg"
style="width:6.26806in;height:2.05972in" />

4.  **Run the Docker Application**

- Build the Docker Image by using

> ***sudo*** ***docker build -t flask-application:1.0.0 .***

<img src="./media/image8.jpeg"
style="width:6.26806in;height:2.05972in" />

<img src="./media/image9.jpeg"
style="width:6.26806in;height:2.05972in" />

- Check if the image built using ***sudo docker images***

<img src="./media/image10.jpeg"
style="width:6.26806in;height:2.05972in" />

5.  R**un the Docker Container**:

- ***docker run -d -p 8000:8000 flask-application:1.0.0***

<img src="./media/image11.jpeg"
style="width:6.26806in;height:2.05972in" />

- Check if the container is running if it is PROCEED to 6 using

<img src="./media/image12.jpeg"
style="width:6.26806in;height:2.05972in" />

6.  Test in Browser Now, go to your browser and access your EC2 public
    IP to check if the app is running properly:
    ***http://18.213.231.230:8000/***

<img src="./media/image13.jpeg"
style="width:5.7093in;height:2.45046in" />

> The page didn’t display successfully. This shows that something is
> wrong. Add the port 8000 to our security group of our instance so do
> that and try accessing the EC2 public IP.

<img src="./media/image14.jpeg"
style="width:6.26806in;height:2.69028in" />

I have successfully deployed the Dockerized Flask app on an AWS EC2
instance. This is a common workflow in modern cloud infrastructure where
applications are containerized for ease of deployment, scalability, and
management.

7.  P**ushing Docker Images to Docker Hub**

After successfully building and running your Docker image, you may want
to share it with others or deploy it to different environments. Docker
Hub is a cloud-based registry service that allows you to store and
distribute Docker images

- Go to [Docker Hub](https://hub.docker.com/)

- Sign up for a free account if you don’t have one already

- Create a repo

<img src="./media/image15.jpeg"
style="width:6.26806in;height:3.02153in" />

- Use the Docker CLI to log in to your Docker Hub account on your
  terminal:

<img src="./media/image16.jpeg"
style="width:6.26806in;height:2.09097in" />

- Tag your image with your Docker Hub username and a repository name.
  The tagging format is:

> ***\<your-dockerhub-username\>/\<repository-name\>:\<tag\>***
>
> ***sudo docker tag flask-application:1.0.0
> smodole/flask-application:1.0.0***

<img src="./media/image17.jpeg"
style="width:5.26336in;height:1.75581in" />

8.  **Push the Image to Docker Hub**

Once your image is tagged, you can push it to Docker Hub using the
following command:

***sudo docker push yourusername/flask-application:1.0.0***

<img src="./media/image18.jpeg"
style="width:6.26806in;height:2.09097in" />

9.  **Verify the Push**

After the push completes, you can verify that your image is on Docker
Hub by visiting your Docker Hub profile and checking the repositories.

<img src="./media/image19.jpeg"
style="width:5.18605in;height:1.89665in" />

<img src="./media/image20.jpeg"
style="width:5.18542in;height:2.76965in" />
