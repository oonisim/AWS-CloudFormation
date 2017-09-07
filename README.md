# AWS-CloudFormation template for 3 tier system.

References
---------------------

* [AWS Basics Using CloudFormation (Reference Demo)](https://github.com/vancluever/aws-basics-using-cloudformation)
* [NAT Instances](http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_NAT_Instance.html)
* [Working with an Amazon RDS DB Instance in a VPC](http://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_VPC.WorkingWithRDSInstanceinaVPC.html)
* [RDS with Cloud Formation and AZ issues](https://stackoverflow.com/questions/33722394/rds-with-cloud-formation-and-az-issues)
* [AWS Resource Types Reference](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-template-resource-type-ref.html)

Overview
---------------------
3 tier system with the front-end Elastic Load Balancer spraying the requests to two web servers. Then Web -> App -> DB.

* Security Groups are filtering accesses between the layers with inbound/outbound rules.
* Connections from inside the VPC go through the NAT gateway, hence no direct IP/socket exposures.
* NAT Gateway functions as the jump box into the instances in the VPC.

![alt text](https://github.com/oonisim/AWS-CloudFormation/blob/master/snapshots/DL.png)

Stack Creation
---------------------
Use **US EAST** regions.

1. In the CloudFormation, [create a new stack](https://us-east-2.console.aws.amazon.com/cloudformation/home?region=us-east-2#/stacks/new).
2. Uplooad the DL3Tier.awscf.json template file.
3. Provide the parameters. KeyPair is the AWS key pair name for the SSH logins to the instances.

![alt text](https://github.com/oonisim/AWS-CloudFormation/blob/master/snapshots/DL.parameters.png)


Verification
---------------------
1. Go to the AWS EC2 console.
2. From the navigation pane on the left, select Load Balancer to get the ELB URL.
![](https://github.com/oonisim/AWS-CloudFormation/blob/master/snapshots/ELBDNS.png)

    Alternatively, identify the ELB resource created from the CloudFormation execution status Resources tab.

    ![](https://github.com/oonisim/AWS-CloudFormation/blob/master/snapshots/CF.Status.Resources.png)

3. Access the ELB URL from the Web browser.
Session Affinity is not configured yet. Keep accessing and will see the different web servers are getting accessed.

    Web 01 server:<br>
    ![](https://github.com/oonisim/AWS-CloudFormation/blob/master/snapshots/ELB2Web01.png)

    Web 02 server:<br>
    ![](https://github.com/oonisim/AWS-CloudFormation/blob/master/snapshots/ELB2Web02.png)

Notes
---------------------
AWS Linux AMI is used for the EC2 instances.

