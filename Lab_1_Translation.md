# LAB: Google Cloud Fundamentals: Getting Started with Compute Engine

## Overview:

In this lab, you will create virtual machines (VMs) and connect to them. You will also create connections between the instances.

## Objectives:

In this lab, you will learn how to perform the following tasks:
    
- Create a Compute Engine virtual machine using the Google Cloud Platform (GCP) Console (Translated to CLI).

- Create a Compute Engine virtual machine using the gcloud command-line interface.

- Connect between the two instances.

## Tasks:

1. Create a Compute Engine virtual machine using the Google Cloud Platform (GCP) Console (Translated to CLI).
    
    1. Configure the GCP Project ID if not already configured

            gcloud config set project [PROJECT_ID]

    2. To view a list of all the zones in a specific region run the following command: gcloud compute zones list | grep [region]

            gcloud compute zones list | grep us-central1

    3. Configure the default GCP zone using the following command followed by the zone you chose: gcloud config set compute/zone [zone]

            gcloud config set compute/zone us-central1-a

    4. Create a virtual machine using the gcloud command line

            gcloud compute instances create "my-vm-1" --machine-type "n1-standard-1" --image-project "debian-cloud" --image "debian-9-stretch-v20190213" --subnet "default" --tags "http"

    5. Create a firewall rule to allow HTTP traffic to the virtual machine
    
            gcloud compute firewall-rules create default-allow-http --direction=INGRESS --priority=1000 --network=default --action=ALLOW --rules=tcp:80 --source-ranges=0.0.0.0/0 --target-tags=http

2. Create a Compute Engine virtual machine using the gcloud command-line interface.
    
    1. To view a list of all the zones in a specific region run the following command: gcloud compute zones list | grep [region]

            gcloud compute zones list | grep us-central1

    2. Change the default zone to another zone for the second virtual machine. For example, if you chose region us-central1 and zone us-central1-a you might choose zone us-central1-b
    
            gcloud config set compute/zone us-central1-b

    3. Create a second virtual machine

            gcloud compute instances create "my-vm-2" --machine-type "n1-standard-1" --image-project "debian-cloud" --image "debian-9-stretch-v20190213" --subnet "default"

3. Connect between the two instances.

    1. To view a list of all the virtual machines associated with a particular GCP Project run
    
            gcloud compute instances list

        You will see the two VM instances you created, each in a different zone. Notice that the Internal IP addresses of these two instances share the first three bytes in common. They reside on the same subnet in their Google Cloud VPC even though they are in different zones.

    2. Connect to my-vm-2

            gcloud compute ssh my-vm-2
    
    3. Use the ping command to confirm that my-vm-2 can reach my-vm-1 over the network

            ping -c 3 my-vm-1

        Notice that the output of the ping command reveals that the complete hostname of my-vm-1 is my-vm-1.c.PROJECT_ID.internal, where PROJECT_ID is the name of your Google Cloud Platform project. GCP automatically supplies Domain Name Service (DNS) resolution for the internal IP addresses of VM instances.

    4. Use the ssh command to open a command prompt on my-vm-1

            ssh my-vm-1
    
        If you are prompted about whether you want to continue connecting to a host with unknown authenticity, enter yes to confirm that you do.

    5. At the command prompt on my-vm-1, install the Nginx web server:

            sudo apt-get install nginx-light -y

    6. Use the nano text editor to add a custom message to the home page of the web server:

            sudo nano /var/www/html/index.nginx-debian.html

    7. Use the arrow keys to move the cursor to the line just below the h1 header. Add text like this, and replace YOUR_NAME with your name:

            Hi from YOUR_NAME

        Press Ctrl+O and then press Enter to save your edited file, and then press Ctrl+X to exit the nano text editor.

    8. Confirm that the web server is serving your new page. At the command prompt on my-vm-1, execute this command:

            curl http://localhost/
    
        The response will be the HTML source of the web server's home page, including your line of custom text.

    9. To exit the command prompt on my-vm-1, execute this command:

            exit

        You will return to the command prompt on my-vm-2

    10. To confirm that my-vm-2 can reach the web server on my-vm-1, at the command prompt on my-vm-2, execute this command:

            curl http://my-vm-1/

        The response will again be the HTML source of the web server's home page, including your line of custom text.

    11. Copy the External IP address for my-vm-1 and paste it into the address bar of a new browser tab. You will see your web server's home page, including your custom text. 

            gcloud compute instances list







