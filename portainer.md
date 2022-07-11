# How to install Portainer on Ubuntu 22.04 and Upgrade existing installations

[brandon.lee](https://www.virtualizationhowto.com/author/john-lee/ "brandon.lee")


If you haven’t heard about Portainer, you need to. It is a great GUI tool that allows installing, configuring, and managing your containers and is much easier to do than using the command line. If you have been following my blog, I just posted a post on how to upgrade your Packer template to work with Ubuntu 22.04 to automate the build of your Ubuntu 22.04 templates in VMware vSphere. Let’s see how to install Portainer on Ubuntu 22.04 and upgrade existing installations.

Read my post on creating an automated installation of Ubuntu 22.04 using Packer:

-   [Packer Build Ubuntu 22.04 for VMware vSphere](https://www.virtualizationhowto.com/2022/04/packer-build-ubuntu-22-04-for-vmware-vsphere/)

## What is Portainer?

Before we look at the steps to install Portainer  [Ubuntu](https://www.virtualizationhowto.com/2021/12/install-pi-hole-in-ubuntu-21-04/) 22.04, let’s consider what it is exactly. As businesses have adopted containers and Kubernetes, there is a tendency to see sprawl in the environment, much like we saw when everyone started virtualizing. If it is easy to spin up a new VM, it is even easier to spin up new containers and Kubernetes clusters simply due to the much smaller size and footprint. A container and Kubernetes management system allow organizations to have a single pane of glass to manage, configure, and operationalize containers and Kubernetes clusters.

However, it doesn’t stop at simple Docker containers and Kubernetes. Portainer is a Universal Container Management System for Kubernetes, Docker/Swarm, and Nomad that simplifies container operations so you can deliver software in a much more agile and flexible way.


## Prerequisites to Install Portainer Ubuntu 22.04

Before we install Portainer Ubuntu 22.04, we will complete the following first steps to get some prerequisites in place:

-   Create a new non-root user in Ubuntu
-   Add the user to sudo users group and remove the requirement for sudo password
-   Install Docker
-   Create a new Docker group and add user to Docker group

To perform the tasks to get the user configured as mentioned, we need to run the following commands:

```
sudo adduser <your username>
usermod -aG sudo <your username>
sudo visudo
```

[![Creating a new user in Ubuntu 22.04](https://www.virtualizationhowto.com/wp-content/uploads/2022/04/Creating-a-new-user-in-Ubuntu-22.04.png)](https://www.virtualizationhowto.com/wp-content/uploads/2022/04/Creating-a-new-user-in-Ubuntu-22.04.png)

Creating a new user in Ubuntu 22.04

Once you enter the  **sudo visudo**  command, you will add the following line at the very bottom of your file. Save and close.

[![Removing the need to enter your sudo password using the user](https://www.virtualizationhowto.com/wp-content/uploads/2022/04/Removing-the-need-to-enter-your-sudo-password-using-the-user.png)](https://www.virtualizationhowto.com/wp-content/uploads/2022/04/Removing-the-need-to-enter-your-sudo-password-using-the-user.png)

Removing the need to enter your sudo password using the user

Next, we install Docker. Here, I am not doing anything special for Ubuntu 22.04, but rather, I am just following the documentation found here:

-   [Install Docker Engine on Ubuntu | Docker Documentation](https://docs.docker.com/engine/install/ubuntu/)

[![Installing Docker in Ubuntu 22.04](https://www.virtualizationhowto.com/wp-content/uploads/2022/04/Installing-Docker-in-Ubuntu-22.04.png)](https://www.virtualizationhowto.com/wp-content/uploads/2022/04/Installing-Docker-in-Ubuntu-22.04.png)

Installing Docker in Ubuntu 22.04

Once Docker is installed, we need to add our non-root user to the Docker user’s group. To do that, we run the following command:

```
sudo usermod -aG docker $USER && newgrp docker
```

[![Adding a user to the Docker users group](https://www.virtualizationhowto.com/wp-content/uploads/2022/04/Adding-a-user-to-the-Docker-users-group.png)](https://www.virtualizationhowto.com/wp-content/uploads/2022/04/Adding-a-user-to-the-Docker-users-group.png)

Adding a user to the Docker users group

## Install Portainer Ubuntu 22.04

Now, we can actually install Portainer in Ubuntu 22.04. This process involves just a couple of steps

-   Creating the volume that Portainer Server will use to store its database
-   Download and install the Portainer Server container

### Create the volume that Portainer Server will use to store its database

```
docker volume create portainer_data
```

### Download and install the Portainer Server container

Next, we will download and install the Portainer container. To do that, use the following:

```
docker run -d -p 8000:8000 -p 9443:9443 --name portainer \
    --restart=always \
    -v /var/run/docker.sock:/var/run/docker.sock \
    -v portainer_data:/data \
    portainer/portainer-ce:2.11.1
```

Once the container image is pulled down, you should be able to navigate to your IP or hostname of your Docker host, port 9443.

[![Install Portainer Ubuntu 22.04](https://www.virtualizationhowto.com/wp-content/uploads/2022/04/Install-Portainer-Ubuntu-22.04.png)](https://www.virtualizationhowto.com/wp-content/uploads/2022/04/Install-Portainer-Ubuntu-22.04.png)

Install Portainer Ubuntu 22.04

[![Configure your admin password for Portainer after installation](https://www.virtualizationhowto.com/wp-content/uploads/2022/04/Configure-your-admin-password-for-Portainer-after-installation.png)](https://www.virtualizationhowto.com/wp-content/uploads/2022/04/Configure-your-admin-password-for-Portainer-after-installation.png)

Configure your admin password for Portainer after installation

[![Viewing the Portainer admin interface](https://www.virtualizationhowto.com/wp-content/uploads/2022/04/Viewing-the-Portainer-admin-interface.png)](https://www.virtualizationhowto.com/wp-content/uploads/2022/04/Viewing-the-Portainer-admin-interface.png)

Viewing the Portainer admin interface

## Custom SSL cert installation

By default, Portainer installs a self-signed certificate with the container. However, you can install a custom SSL certificate during installation or using the GUI after installation. Below is the syntax to pass in the needed parameters for installing a proper certificate when provisioning the container:

```
docker run -d -p 8000:8000 -p 9000:9000 -p 9443:9443 \
    --name=portainer --restart=always \
    -v /var/run/docker.sock:/var/run/docker.sock \
    -v portainer_data:/data \
    portainer/portainer-ce:2.11.1 \
    --sslcert /path/to/cert/portainer.crt \
    --sslkey /path/to/cert/portainer.key
```

## Upgrade existing Portainer installation

On the official Portainer install documentation, they have not yet updated their documented install example to include the latest installation. Instead, it is still showing 2.9.3. If you have installed an older version, you can easily upgrade. Below are the steps.

-   Stop the existing Portainer Docker  [container](https://www.virtualizationhowto.com/2022/01/virtualization-vs-containerization-in-2022-home-lab/)
-   Remove the existing container (don’t panic, your data is in the volume created during install)
-   Pull the latest container (2.11.1 at time of writing)
-   Run the new Docker container

You can see the steps performed in the screenshot below:

[![Upgrading Portainer Server installation in Ubuntu 22.04](https://www.virtualizationhowto.com/wp-content/uploads/2022/04/Upgrading-Portainer-Server-installation-in-Ubuntu-22.04.png)](https://www.virtualizationhowto.com/wp-content/uploads/2022/04/Upgrading-Portainer-Server-installation-in-Ubuntu-22.04.png)

Upgrading Portainer Server installation in Ubuntu 22.04

After performing the steps listed, you should be able to browse back out to the Portainer IP or hostname to port 9443.

[![Enhanced GUI in Portainer 2.11.1](https://www.virtualizationhowto.com/wp-content/uploads/2022/04/Enhanced-GUI-in-Portainer-2.11.1.png)](https://www.virtualizationhowto.com/wp-content/uploads/2022/04/Enhanced-GUI-in-Portainer-2.11.1.png)

Enhanced GUI in Portainer 2.11.1

## Portainer install Ubuntu 22.04 FAQs

-   **How to install Portainer on Ubuntu 22.04?**  To install Portainer on Ubuntu 22.04, you need to create the storage to house the database and pull the Portainer container. As I have demonstrated in the post, there are other steps you need to accomplish to get ready for the Portainer installation, such as installing Docker, etc.
-   **Is Portainer free?**  Yes, the community edition is free. They have a Business Edition that is a paid product. However, you can also use the Business Edition for up to 5 free nodes.
-   **Does Portainer support Kubernetes?** Yes, Docker, Kubernetes, and Nomad clusters are supported.
-   **How do you upgrade Portainer?**  The Portainer upgrade process is straightforward. Due to the database being stored in a persistent location, you can simply remove the existing Portainer container and then pull and run the upgraded Portainer container.

## Wrapping Up

How to install Portainer on Ubuntu 22.04 and upgrade existing installations is very simple. Portainer has designed the installation and upgrade process to be seamless and they have thought through how to deal with the data of existing installations. With a few simple commands, you can be up and running with Portainer in no time. Stay tuned for a deeper dive into using Portainer.
