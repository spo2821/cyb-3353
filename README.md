To start the install for the Docker, we need to first open up our Ubuntu Virtual Machine that we created earlier this semester. From here, we will be working to create our Docker install and install OpenVAS.

We'll need to first go into our Ubuntu VM and open up a new terminal instance. From here we'll run the command "sudo apt update" to update our apt installer to ensure that everything is up to date.

From here we will then run "sudo apt install docker.io", this command is what actually installs our docker instance which is what will allow us to complete this lab. 

For me, running this command didn't initially work, and I had to do a lot troubleshooting to get my apt installer to install the right packages, and I eventually had to run the command "sudo apt install ca-certificates curl gnupg lsb-release", this would allow me to run the docker install properly.

From here, I ran "curl -fsSl https://download.docker.com/linux/ubuntu/gpg | sudo gpg -dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg", this worked to allow the docker install to continue.

From here I needed to further the docker install and run the commands "sudo apt install docker-ce docker-ce-cli containerd.io" to finish the install.

From here, we had to assign docker admin permissions so that it could properly create containers and work, and to do this we had to run "sudo usermod -aG docker admin" so that it would work properly. 
