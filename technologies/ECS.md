# ECS

We have chosen to run our workloads on [ECS](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/Welcome.html) which is a container management service. There are two options when it comes to choosing the servers upon which the containers are run: EC2 or Fargate. EC2 requires spinning up, configuring and maintaining your own which gives more flexibility but at the expense of overhead maintenance. AWS Fargate allows you to run containers without havinng to manage servers or clusters of EC2 instances. We had decided on using AWS Fargate to remove that additional overhead.

## Architecture

As the application is quite simple, there is a 3-tier network architecture (public, private, database). The [web-application service](https://github.com/tintulip/web-application) runs within the private subet tier and listens on port `8080`. A load balancer forwards traffic from port `443` to the web-application by default.

## Security Groups

[Security Groups](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_SecurityGroups.html) are used to act as virtual firewalls and the ECS service will have security groups defined as part of its network configuration. The security groups are set up so that tcp traffic on port `443` is allowed to the loadbalancer from the outside world. Traffic that originated from that security group is only allowed to use port `8080` to talk to the web-application and in turn the traffic from the web-application security group can only use port `5432` to talk to the database.

However, when a ECS service starts a task, it needs to talk to services such as AWS Secrets Manager (for the DB password) and AWS ECR (for the container image) but uses `https` to do so. As the security group does not allow `https` traffic to the outside world from within the web-application, another solution needs to be found to allow services to talk to other AWS services without leaving the Amazon network.

## VPC Endpoints

[VPC endpoints](https://docs.aws.amazon.com/vpc/latest/privatelink/vpc-endpoints.html) are the solution to the above. It is a technology that enables services to privately access other services by using a private IP address and public IP addresses are not required to communicate.

With ECS, the services that are interacted with include [ECR (dkr and api)](https://docs.aws.amazon.com/AmazonECR/latest/userguide/vpc-endpoints.html), [Secrets Manager](https://docs.aws.amazon.com/secretsmanager/latest/userguide/vpc-endpoint-overview.html) and [CloudWatch logs](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/cloudwatch-logs-and-interface-VPC.html).

Each of the above are [interface endpoints](https://docs.aws.amazon.com/vpc/latest/privatelink/vpce-interface.html) which is an elastic network interface with a private IP address from within the private subnet range provided. It servces as the entry point for traffic destined to its corresponding AWS service. Security groups can also be attached to the VPC endpoints and each of these VPC endpoints has an `ingress` rule that allows traffic from port `443` in that originated from the service security group.

