To start the install for the Docker, we need to first open up our Ubuntu Virtual Machine that we created earlier this semester. From here, we will be working to create our Docker install and install OpenVAS.

We'll need to first go into our Ubuntu VM and open up a new terminal instance. From here we'll run the command "sudo apt update" to update our apt installer to ensure that everything is up to date.

From here we will then run "sudo apt install docker.io", this command is what actually installs our docker instance which is what will allow us to complete this lab. 

For me, running this command didn't initially work, and I had to do a lot troubleshooting to get my apt installer to install the right packages, and I eventually had to run the command "sudo apt install ca-certificates curl gnupg lsb-release", this would allow me to run the docker install properly.

From here, I ran "curl -fsSl https://download.docker.com/linux/ubuntu/gpg | sudo gpg -dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg", this worked to allow the docker install to continue.

From here I needed to further the docker install and run the commands "sudo apt install docker-ce docker-ce-cli containerd.io" to finish the install.

From here, we had to assign docker admin permissions so that it could properly create containers and work, and to do this we had to run "sudo usermod -aG docker admin" so that it would work properly. 

At this point in my install, I ran into an issue with Ubuntu that required me to restart the process and reinstall curl, which set me back quite a bit. Eventually I was able to get it to work, and had to continue with the command "sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose" 

And then I had to give executable permissions to the docker-compose file by running "sudo chmod +x /usr/local/bin/docker-compose", which allowed that permission to the file. 

Next I was able to test the docker-compose was working by running "docker-compose --version" which printed out the version I installed to ensure that the command worked. 

I then was able to test that docker was working by running a new container with "sudo docker run hello-world" and indavertently would set myself back in the future as this container along with another one I tested pulled too many resources and would have to be deleted later. 

From here we need to pull the correct docker image so that we can set up a new container, and we do this by running "sudo docker pull ubuntu", and we can check to make sure it's installed by running "sudo docker images" and checking from there. We can then run it by running "sudo docker run -it ubuntu" and check to make sure it's running by running the command "sudo docker ps -a", and "sudo docker ps -l".

From here, we kind of have docker set up, and now we will need to continue into the OpenVAS installation, this is where I ran into an issue with a container I set up in my installation here to test docker, and the hello-world container, and I had to remove them using the command "sudo docker rm hello-world".

From here we need to create the container for openvas, and we can do this by running the command "sudo docker run -d -p 433:433 --name openvas mikesplain/openvas"

Now we can actually oepn openvas, and to do this we need to go to a web browser and type in "https://localhost:433", and then log into the web page using "admin" as the username and the password. The dashboard should look like this image when the log in is complete. <img width="1123" alt="Screen Shot 2021-12-02 at 10 58 41 PM" src="https://user-images.githubusercontent.com/19178865/144547843-28701615-c0b4-403d-99ca-67901718dbae.png">

After you log in, you have to run a scan by clicking on the Scans menu and then clicking on Tasks, from this page, you will need to click on the purple icon and select Task Wizard, and then click "Start Scan", after this step has occured, you should see a scan start at the bottom that looks like the following image. 
 
 <img width="952" alt="Screen Shot 2021-12-02 at 10 56 16 PM" src="https://user-images.githubusercontent.com/19178865/144547768-1e6718a6-a2b5-457d-a3d2-8cea0bdc4da2.png">

<h1>Sources Used </h1>

https://docs.docker.com/compose/install/

https://github.com/mikesplain/openvas-docker
