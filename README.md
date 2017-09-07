# AWS-CloudFormation (CF) template for 3 tier system.

References
---------------------

* [AWS Basics Using CloudFormation (Reference Demo)](https://github.com/vancluever/aws-basics-using-cloudformation)
* [NAT Instances](http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_NAT_Instance.html)
* [Working with an Amazon RDS DB Instance in a VPC](http://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_VPC.WorkingWithRDSInstanceinaVPC.html)
* [RDS with Cloud Formation and AZ issues](https://stackoverflow.com/questions/33722394/rds-with-cloud-formation-and-az-issues)
* [AWS Resource Types Reference](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-template-resource-type-ref.html)

Overview
---------------------
CF template to create a sample 3 tier system with the front-end Elastic Load Balancer spraying the requests to two web servers.

* Front end access is load balanced with the Elastic Load Balancer (ELB).
* Front end High Availability (HA) is with the web cluster and session affinity.
* All instances are contained in a VPC. Only HTTP/S and SSH are allowed.
* Security Groups filters connections between the layers with inbound/outbound rules.
* Connections from inside the VPC can go only through the NAT gateway, hence no direct IP/socket exposures.
* NAT Gateway functions as the jump box (only SSH from a specified IP allowed) to the instances in the VPC.
* Unless the known defaults (e.g 3306 for MySQL port), all configurable values are parameterised. <BR>

HTTP/S outbound accesses are allowed from the instances for the package installations via yum (apt). An internal package repository should be setup, however for the sample system.

![alt text](https://github.com/oonisim/AWS-CloudFormation/blob/master/snapshots/DL.png)

Stack Creation
---------------------
**US EAST** regions need to be used.

1. In the CloudFormation, [create a new stack](https://us-east-2.console.aws.amazon.com/cloudformation/home?region=us-east-2#/stacks/new).
2. Uplooad the DL3Tier.awscf.json template file.
3. Provide the parameters. KeyPair is the AWS key pair name for the SSH logins to the instances.

![alt text](https://github.com/oonisim/AWS-CloudFormation/blob/master/snapshots/DL.parameters.png)

4. Confirm the successful completion.

    Note the ELB resource created in the CloudFormation execution status Resources tab to get the ELB URL later.

    ![](https://github.com/oonisim/AWS-CloudFormation/blob/master/snapshots/CF.Status.Resources.png)

Verification
---------------------
1. Go to the AWS EC2 console.
2. From the navigation pane on the left, select Load Balancer to get the ELB URL.
![](https://github.com/oonisim/AWS-CloudFormation/blob/master/snapshots/ELBDNS.png)

3. Access the ELB URL from the Web browser.
Session Affinity is not configured yet. Keep accessing and will see the different web servers are getting accessed.

    Web 01 server:<br>
    ![](https://github.com/oonisim/AWS-CloudFormation/blob/master/snapshots/ELB2Web01.png)

    Web 02 server:<br>
    ![](https://github.com/oonisim/AWS-CloudFormation/blob/master/snapshots/ELB2Web02.png)

4. Copy the SSH private key of the Key Pair to the NAT instance to be able to SSH into other instances.
5. SSH login to the NAT instance with the Key Pair.

    <pre><code>
    oonisim@:~/.ssh$ scp ec2.pem ec2-user@13.59.1.71:~/.ssh/
    oonisim@:~/.ssh$ ssh ec2-user@13.59.1.71
    </code></pre>

6. Run "sudo yum -y update to verify HTTP/S outbound rules are configured as expected."

    <pre><code>
    [ec2-user@ip-10-0-0-198 ~]$ ssh 10.0.2.156  <--- SSH login to an App instance.

           __|  __|_  )
           _|  (     /   Amazon Linux AMI
          ___|\___|___|
    https://aws.amazon.com/amazon-linux-ami/2017.03-release-notes/

    [ec2-user@ip-10-0-2-156 ~]$ sudo yum update
    Loaded plugins: priorities, update-motd, upgrade-helper
    amzn-main                                                                                                                                                                                    | 2.1 kB  00:00:00
    amzn-updates                                                                                                                                                                                 | 2.3 kB  00:00:00
    (1/5): amzn-main/latest/group                                                                                                                                                                |  35 kB  00:00:00
    (2/5): amzn-updates/latest/group                                                                                                                                                             |  35 kB  00:00:00
    (3/5): amzn-updates/latest/updateinfo                                                                                                                                                        | 414 kB  00:00:00
    (4/5): amzn-main/latest/primary_db                                                                                                                                                           | 3.6 MB  00:00:00
    (5/5): amzn-updates/latest/primary_db                                                                                                                                                        | 672 kB  00:00:00
    Resolving Dependencies
    --> Running transaction check
    ...
    </code></pre>

7. Verify the MySQL RDS instance from the RDS console.
    ![MySQL](https://github.com/oonisim/AWS-CloudFormation/blob/master/snapshots/RDSInsance.png)

8. Verify the MySQL connection (port 3306) is open from an App instance.
    <pre><code>
    [ec2-user@ip-10-0-2-156 ~]$ telnet drs41f9esu2lp7.cfebzse2r1lv.us-east-2.rds.amazonaws.com 3306
    Trying 10.0.4.222...
    Connected to drs41f9esu2lp7.cfebzse2r1lv.us-east-2.rds.amazonaws.com.
    Escape character is '^]'.
    </code></pre>


Notes
---------------------
AWS Linux AMI is used for the EC2 instances.


