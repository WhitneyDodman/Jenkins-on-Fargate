# Jenkins ECS!

This modules creates the infrastructure for running Jenkins on ECS/Fargate.


# Variables

|Variable                        | Description                                          |
|--------------------------------|------------------------------------------------------|
|`VPC`            				 | The VPC to deploy the resources                       |
|`Subnets`                       | Subnets for ECS and EFS. (Should be PRIVATE subnets)  |
|`Cluster`                       | The short name or full Amazon Resource Name (ARN) of the cluster that you run your service on. If you do not specify a cluster, the default cluster is assumed. This cluster must already exist.  |
|`ECRImage`                      | The image used to start a container. This string is passed directly to the Docker daemon. This image must already exists |
|`ServiceName`                   | The name of your service. Up to 255 letters (uppercase and lowercase), numbers, underscores, and hyphens are allowed. Service names must be unique within a cluster, but you can have similarly named services in multiple clusters within a Region or across multiple Regions. |
|`ELBSubnets`                    | Subnets for the ELB. Should be PUBLIC subnets if you want to be able to access the Jenkis site from the Internet. |
|`S3LoggingBucket`               | Name of the S3 bucket to log ELB requests. This bucket should already exist. |
|`CWBurstCreditThreshold`        | The minimum EFS Burst Credit level before generating an alarm. |
|`CWBurstCreditPeriod`           | The number of periods over which the EFS Burst Credit level is compared to the specified threshold. |
|`SSLCertificateArn`             | The arn of the SSL certificate to attache to the ELB. This certificate must be prexisting |
|`Scheme`                        | Indicates whether the load balancer is Internet-facing or internal. |
|`HostedZoneName`                | The DNS domain name for the environment. (e.g. ci.bigassfans.com) |
|`HostedZoneId`                  | The DNS Zone Id that matches the domain name. |

## Creating the Docker Image

Use the following commands to create and upload the docker to ECR. The examples below assume the ECR Repository is called ecs-jenkins:

Login to the ECR repo with the AWS client and pass credentials to docker
> aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin <account_numer>.dkr.ecr.us-east-1.amazonaws.com

Build the Docker Image
> docker build -t  <account_numer>.dkr.ecr.us-east-1.amazonaws.com/ecs-jenkins:latest ./docker/

Push Docker Image to ECR
> docker push <account_numer>.dkr.ecr.us-east-1.amazonaws.com/ecs-jenkins:latest
