# Working with EC2 Instances

In this activity you will:
1. Create Public EC2 Instance
2. Install Web Server (nginx, apache, etc.) in Public EC2 Instance
3. Add policies to give access to Nginx
4. Create Private EC2 Instance
5. Download Updates in Private EC2 Instance
6. Create and restore EC2 AMI's


## Create an EC2 Instance

 1. Log into the AWS Console and under Services select EC2.
 2. Select Instances on the left menu.
 3. Click on “Launch Instances”
 4. Select the AMI Image that you want to use for your Instance _(please note that this has a cost, if you have an account with less than a year of creation you can look for the legend “free tier eligible” to avoid charges)._
 5. Select the Instance type that you want to deploy (please note that this has a cost, if you have an account with less than a year of creation you can look for the legend “free tier eligible” to avoid charges).
 6. Instance details and storage can be left as is, unless told otherwise by the faculty.
 7. Add the tags:
	 8. Owner: <your EID>
	 9. Project: DevOpsAcademy
 8. Rename the security group with your EID.
	 11. your_EID_sec_group
 9. Click on review and launch and then launch.
	 13. Select your key pair, if you don’t have one, create it naming it with your EID and keep it somewhere safe.


## Install Web Server

 1. Open an SSH client.
 2. Locate your private key file.
 3. If necessary, to ensure your key is not publicly viewable, run.
	 4. chmod 400 <youraccesskey>.pem
 4. Log in to your Instance
	 5. Select your instance and click on connect
	 6. Pick your preferred connection method and login to the Instance
 5. Install and run your webserver
	 6. sudo yum install -y httpd
	 7. chkconfig httpd on
	 8. sudo  systemctl start httpd
 6. Try opening the webserver in a browser by accessing with the Instance public IP. 
>		What happened?
  8. Open the security group attached to your instance and under Inbound Rules select “edit inbound rules”
 8. Click on “Add Rule”
	 9. Type – HTTP
	 10. Source – My IP
 9. Get the public IP of your instance and paste it in your browser to validate if the webserver is running.
 10. Go to /var/www/html and create a new file ‘index.html’ and edit it any way you want.
 11. Refresh the browser window where you have the webserver open.

## Create an AMI

 1. On AWS console instance list go to the ‘Actions’ button, select ‘Image and templates’ and click on ‘Create Image’
 2. Fill out the new image information:
	 3. Image name: <yourEID>_AMI
	 4. Description: Optional unless your instructor tells you otherwise
	 5. Owner: <your EID>
	 6. Project: DevOpsAcademy

## Create a Load Balancer

 1. On the left side menu go to Load Balancing -> Load Balancers and click on “Create Load Balancer”, for the type, select Application Load Balancer
 2. Load Balancer Name: <yourEID>-LoadBalancer
 3. Load Balancer Scheme: internet-facing
 4. Select any two subnets in different AZs (make sure they are internet facing)
 5. Select the Security Group you previously created.
 6. Create a new target group and name it <yourEID>-targetgroup
 7. Leave the Register Targets section blank as this will be attached later


## Create an Auto Scaling Group

 1. On the left side menu go to Auto Scaling -> Auto Scaling Groups and click on “Create Auto Scaling Group”, for the type, select Application Load Balancer 
 2. Auto Scaling group name: <yourEID>-AutoScalingGroup 
 3. Make sure you select the Launch Configuration option and select “Create a launch configuration”

## Create a Launch Configuration

 1. For the Launch Configuration select: 
	 2. Launch Configuration Name: <yourEID>-LaunchConfig
	 3. AMI Image: the AMI you created from your webserver instance
	 4. Instance type: t2.micro
	 5. Security Group: the one you previously created


## Create an Auto Scaling Group - part 2

 1. Go back to the Auto Scaling window and next to the launch configuration click on the refresh button. 
 2. Select the newly created Launch Configuration. 
 3. Select the same VPC and Subnets you selected while creating the Load Balancer. 
 4. For the load balancing select:
	 5. Attach to an existing load balancer 
	 6. Choose from your load balancer target group 
	 7. Select the target group you previously created 
 8. Select health checks for the ELB 
 9. Change the health check grace period to 30
 10. Change the group size to 2. 
 11. Add the tags: 
	 12. Owner: <yourEID> 
	 13. Project: DevOpsAcademy 
	 14. Check that the Auto Scaling group was created and is active.

# Final Check

1.Go to the Instances and try to access the webserver via the public IPs.

> What happened?

2.Go to Load Balancers and get the DNS Name, try to access the webserver this way.

> What happened?

1. Go to the Security Groups section and select your security group.
2. Click on edit inbound rules.
3. Add a new rule allowing HTTP (port 80) traffic from the Security Group itself.
4. Refresh the load balancer window.

> What happened?


 1. Go to the Instances section and terminate an instance.
 2. Select the instance to terminate
 3. Instance state
 4. Terminate Instance
> What happened?
>Can you still access the webserver via the Load Balancer?



# Clean Up


Go to the Load Balancer section
	
	Select the Load Balancer -> Actions -> Delete
Go to the Target Groups section
	
	Select the Target Group -> Actions -> Delete
Go to the Auto Scaling Groups section
	
	Select the Auto Scaling Group -> Delete
Go to the Launch Configuration section

	Select the Launch Configuration -> Actions -> Delete
Go to the Instances section and validate they are terminated.
Go to the AMIs section

	Select your AMI -> actions -> deregister
Go to the Snapshots section

	Select your AMI snapshot -> Actions -> Delete
