# AWS-CloudFormation template for 3 tier system.

References
---------------------

* [AWS Basics Using CloudFormation (Reference Demo)](https://github.com/vancluever/aws-basics-using-cloudformation)
* [AWS Resource Types Reference](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-template-resource-type-ref.html)
* [Working with an Amazon RDS DB Instance in a VPC](http://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_VPC.WorkingWithRDSInstanceinaVPC.html)
* [RDS with Cloud Formation and AZ issues](https://stackoverflow.com/questions/33722394/rds-with-cloud-formation-and-az-issues)

Overview
---------------------
3 tier system with the front-end Elastic Load Balancer spraying the requests to two web servers. Then Web -> App -> DB. Access to the gateway, load balancer -> web, web -> app, app -> db are filtered with the security groups configurations with inbound/outbound rules.

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
2. From the navigation pane on the left, select Load Balancer.
![](https://github.com/oonisim/AWS-CloudFormation/blob/master/snapshots/ELBDNS.png)

    Alternatively, from the CloudFormation execution status Resources tab, identify the ELB resource created, and follow the link.

    ![](https://github.com/oonisim/AWS-CloudFormation/blob/master/snapshots/CF.Status.Resources.png)

3. Copy the DNS name of the ELB and access from the Web browser.
Session Affinity is not configured yet. keep accessing and will see the different web servers are getting accessed.

    Web 01 server:

    ![](https://github.com/oonisim/AWS-CloudFormation/blob/master/snapshots/ELB2Web01.png)

    Web 02 server:<br>

    ![](https://github.com/oonisim/AWS-CloudFormation/blob/master/snapshots/ELB2Web02.png)