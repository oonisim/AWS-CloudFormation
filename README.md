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

![alt text](https://github.com/oonisim/AWS-CloudFormation/blob/master/DL.png)

Creating Stack
---------------------
In US EAST regions, from the CloudFormation designer, (create a new stack](https://us-east-2.console.aws.amazon.com/cloudformation/home?region=us-east-2#/stacks/new) wiht the DL.json template file.load the template