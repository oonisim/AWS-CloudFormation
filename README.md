# AWS-CloudFormation template for 3 tier system.

References
---------------------
https://github.com/vancluever/aws-basics-using-cloudformation

Author
---------------------
Masayuki Onishi (modifed the original in reference)

Overview
---------------------
ELB (Elastic Load Balancer x 2) -> Web -> App -> DB

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
(https://github.com/oonisim/AWS-CloudFormation/blob/master/snapshots/ELBDNS.png)

Alternatively, from the CloudFormation execution status Resources tab, identify the ELB resource created, and follow the link.
(https://github.com/oonisim/AWS-CloudFormation/blob/master/snapshots/CF.Status.Resources.png)

3. Copy the DNS name of the ELB and access from the Web browser.
Session Affinity is not configured yet. keep accessing and will see the different web servers are getting accessed.

Web 01 server:
(https://github.com/oonisim/AWS-CloudFormation/blob/master/snapshots/ELB2Web01.png)

Web 02 server:
(https://github.com/oonisim/AWS-CloudFormation/blob/master/snapshots/ELB2Web02.png)