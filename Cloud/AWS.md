AWS vs GCP

Every Google Cloud Run container is assigned an URL like [https://my-apache-8cqci21j0m-lz.a.run.app](https://my-apache-8cqci21j0m-lz.a.run.app). This URL is the only way to access the app

в AWS можно вешать кастомный домен на API Gateway, в гугле нельзя

Everything in aws is API call

AWS IaaC: infrastructure as a code

S3 (simple storage service)

DynamoDB: aka mongoDB

EC2 (Elastic Compute Cloud)

EC2 Auto Scaling

ELB (Load Balancing)

ALB

IAM (Identity and Access Management): IAM is a web service that enables you to manage access to your AWS account and resources. It also provides a centralized view of who and what are allowed inside your AWS account (authentication), and who and what have permissions to use and work with your AWS resources (authorization). Apply iam policies to grant or deny access to take action(Everything in aws is API call)

RDS

CloudWatch

Lambda  
Batch  
SAM: (Serverless Application Model) is an open-source framework for building serverless applications. 

VPC:  
CloudFormation stack:

Fargate: is a Container as a Service (CaaS)

Lambda: is a Function as a Service (FaaS)

[https://www.simform.com/blog/aws-fargate-vs-lambda/](https://www.simform.com/blog/aws-fargate-vs-lambda/)

ECS manages and deploys Docker containers

EKS manages and deploys Kubernetes containers

ECR (Elastic Container Registry): the AWS Docker repository

 if you explore ECS or EKS with the [EC2](https://aws.amazon.com/ec2/) launch type, you must create many configurations and manage the underlying infrastructure. Also, familiarizing yourself with numerous infrastructure components and resources is necessary. That’s where AWS Fargate comes into play.

[Fargate](https://aws.amazon.com/fargate/) is a serverless compute engine that allows you to run containers without managing the infrastructure. It complements ECS/EKS and makes launching container-based applications much more effortless.

[Lambda](https://aws.amazon.com/lambda/), an event-driven compute engine, is another serverless technology from AWS. It reduces the operational overhead of managing the infrastructure with a pay-per-use model and no upfront cost. In addition, it enables you to run code in response to events and automatically provisions for and manages the compute resources required.

Fargate: Typically, the startup time for Fargate containers is 60-90 seconds which is more than Lambda. However, Fargate has dedicated resources and no runtime limitations, so the environment remains in a warm state. But it may take more startup time if there is a sudden spike of requests, and it takes longer to scale up.

Lambda: On the other hand, the initial Lambda startup takes 5 seconds, following which the same functions have a negligible startup time. But a Lambda function stops running after 15 minutes until triggered by an event, causing cold starts. However, there are multiple ways and techniques to reduce and prevent Lambda cold starts that you can read in our blog post on [how to avoid Lambda cold starts.](https://www.simform.com/blog/lambda-cold-starts/)

Fargate: It comes with additional complexity as you still have to register your containers with ECS. But it offers significantly more flexibility in that you have full access to the configuration of each container.

Fargate: In the case of Fargate, you need to set up autoscaling. Also, you need to shut down Fargate tasks on a manual or scheduled basis as it cannot scale to 0, adding to maintenance. Moreover, in Fargate, the developers must keep the base container images updated. So it again adds to the maintenance but allows for more control. However, it immediately adds tasks and allocates compute power required automatically, adding capacity in a stair-step manner.

Lambda: Lambda functions are scalable by design, so they automatically launch instances to meet the increases in demand. Apart from rapid auto-scaling, it can also scale to 0. So, you don’t have to pay for idle applications, which is useful for low traffic workloads. This ability of Lambda to scale from 0-1000 rapidly is essential for spiky and unpredictable traffic. Moreover, you can spin architectures easily with Lambda as it requires only simple functions and application setup and maintenance.

Cloud Computing Models

Infrastructure as a Service (IaaS)

Infrastructure as a Service (IaaS) contains the basic building blocks for cloud IT and typically provides access to networking features, computers (virtual or on dedicated hardware), and data storage space. IaaS provides you with the highest level of flexibility and management control over your IT resources and is most similar to existing IT resources that many IT departments and developers are familiar with today.

Platform as a Service (PaaS)

Platform as a Service (PaaS) removes the need for your organization to manage the underlying infrastructure (usually hardware and operating systems) and allows you to focus on the deployment and management of your applications. This helps you be more efficient as you don’t need to worry about resource procurement, capacity planning, software maintenance, patching, or any of the other undifferentiated heavy lifting involved in running your application.

Software as a Service (SaaS)

Software as a Service (SaaS) provides you with a completed product that is run and managed by the service provider. In most cases, people referring to Software as a Service are referring to end-user applications. With a SaaS offering you do not have to think about how the service is maintained or how the underlying infrastructure is managed; you only need to think about how you will use that particular piece of software. A common example of a SaaS application is web-based email which you can use to send and receive email without having to manage feature additions to the email product or maintain the servers and operating systems that the email program is running on.

Cloud Computing Deployment Models

Cloud

A cloud-based application is fully deployed in the cloud and all parts of the application run in the cloud. Applications in the cloud have either been created in the cloud or have been migrated from an existing infrastructure to take advantage of the [benefits of cloud computing](https://aws.amazon.com/what-is-cloud-computing/). Cloud-based applications can be built on low-level infrastructure pieces or can use higher level services that provide abstraction from the management, architecting, and scaling requirements of core infrastructure.

Hybrid

A hybrid deployment is a way to connect infrastructure and applications between cloud-based resources and existing resources that are not located in the cloud. The most common method of hybrid deployment is between the cloud and existing on-premises infrastructure to extend, and grow, an organization's infrastructure into the cloud while connecting cloud resources to the internal system. For more information on how AWS can help you with your hybrid deployment, visit our [Hybrid Cloud with AWS](https://aws.amazon.com/hybrid/) page.

On-premises

The deployment of resources on-premises, using virtualization and resource management tools, is sometimes called the “private cloud.” On-premises deployment doesn’t provide many of the benefits of cloud computing but is sometimes sought for its ability to provide dedicated resources. In most cases this deployment model is the same as legacy IT infrastructure while using application management and virtualization technologies to try and increase resource utilization. For more information on how AWS can help, see [Use case: Cloud services on-premises](http://aws.amazon.com/hybrid/use-cases/#Use_case.3A_Cloud_services_on-premises).

The AWS Cloud infrastructure is built around AWS Regions and Availability Zones. An AWS Region is a physical location in the world where we have multiple Availability Zones.

AWS Regions are independent from one another. This means that your data is not replicated from one Region to another, without your explicit consent and authorization.

AWS Regions are clusters of Availability Zones that are connected through highly availably and redundant high-speed links and Availability Zones are clusters of data centers that are also connected through highly available and redundant high-speed links.

us-east-1c: us-east-1 - region; c - availability zone

[https://aws.amazon.com/about-aws/global-infrastructure/regional-product-services/](https://aws.amazon.com/about-aws/global-infrastructure/regional-product-services/)

# # Deployers # #

Serverless Framework: The Serverless Framework was designed to provision your AWS Lambda Functions

# # Access to AWS account # #

[https://docs.amazonaws.cn/en_us/IAM/latest/UserGuide/id.html](https://docs.amazonaws.cn/en_us/IAM/latest/UserGuide/id.html)

Root user

IAM user

Roles # used for access between different aws services

Groups

Policies

# # Access Policy # #

{

    "Version": "2012-10-17",

    "Statement": [

        {

            "Sid": "Statement1",

            "Effect": "Allow", # Allow or Deny

            "Principal": {

                "AWS": "*"

            },

            "Action": "s3:*",

            "Resource": "arn:aws:s3:::kodit-platform"

        }

    ]

}

- The Version element defines the version of the policy language. It specifies the language syntax rules that are needed by AWS to process a policy. To use all the available policy features, include "Version": "2012-10-17" before the "Statement" element in all your policies.
- The Effect element specifies whether the statement will allow or deny access. In this policy, the Effect is "Allow", which means you’re providing access to a particular resource.
- The Action element describes the type of action that should be allowed or denied. In the above policy, the action is "*". This is called a wildcard, and it is used to symbolize every action inside your AWS account.
- The Resource element specifies the object or objects that the policy statement covers. In the policy example above, the resource is also the wildcard "*". This represents all resources inside your AWS console.

arn:aws:s3:eu-central-1:*:*

ARN:PARTITION:S3:REGION:AZ:BUCKET-NAME

[https://aws-tc-largeobjects.s3-us-west-2.amazonaws.com/DEV-AWS-MO-GCNv2/lab-2-iam.html](https://aws-tc-largeobjects.s3-us-west-2.amazonaws.com/DEV-AWS-MO-GCNv2/lab-2-iam.html)

|   |   |
|---|---|
|{<br><br>  "resource": "/api/auth/v2/{proxy+}",<br><br>  "path": "/api/auth/v2/auth/google",<br><br>  "httpMethod": "GET",<br><br>  "headers": {<br><br>    "Content-Type": "application/json"<br><br>  },<br><br>  "requestContext": {}<br><br>}||
|{<br><br>  "resource": "/api/auth/v2/{proxy+}",<br><br>  "path": "/api/auth/v2/rbac/validate",<br><br>  "httpMethod": "POST",<br><br>  "headers": {<br><br>    "Content-Type": "application/json",<br><br>    "Authorization": "a"<br><br>  },<br><br>  "body": "{\"method\": \"patch\", \"endpoint\": \"/api/opportunity/123/inbound/opportunities/123\", \"token\": \"a\"}",<br><br>  "requestContext": {}<br><br>}||
|||

======EC2======

chmod 400 keyfile.pem # change permissions

ssh-add keyfile.pem # add to ssh agent - optional

ssh-add --apple-use-keychain keyfile.pem # add to keychain - optional

ssh -i keyfile.pem ubuntu@[3.229.67.17](about://#ElasticIpDetails:PublicIp=3.229.67.17) # connect to server

sudo passwd root # change password

su root

ssh-keygen -t rsa -b 4096 -C "ruslan.konovalov@metabolism.cloud" # create certificate on local machine

add id_rsa.pub to authorized_keys on the server

======Cost Management======

[https://kelion.medium.com/оптимизируем-расходы-на-amazon-aws-81354d27dfaa](https://kelion.medium.com/оптимизируем-расходы-на-amazon-aws-81354d27dfaa)

Выключать dev/staging по расписанию

Использовать тип spot

Для prod использовать reversed

======S3======

aws s3 cp s3://kodit-platform-integration-production/idealista/  . --recursive

======Cloud Watch======

fields @timestamp, @message

| filter @message like /elenamarcos1982@gmail.com/

| sort @timestamp desc

| limit 20

======Permissions======

[https://docs.aws.amazon.com/AmazonS3/latest/API/API_Operations_Amazon_Simple_Storage_Service.html](https://docs.aws.amazon.com/AmazonS3/latest/API/API_Operations_Amazon_Simple_Storage_Service.html)


API Gateway (provides load balancing, reverse proxy) has less features then Nginx

Application Load Balancer + Nginx

REST V1 api - старая версия API Gateway; никто не пользуется, много настроек

HTTP V2 httpApi - новая версия API Gateway; упрощенная

[https://www.serverless.com/framework/docs/providers/aws/events/http-api](https://www.serverless.com/framework/docs/providers/aws/events/http-api)

[https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-policy-templates.html](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-policy-templates.html)

API Gateway supports multiple mechanisms for controlling and managing access to your HTTP API:

• Lambda authorizers use Lambda functions to control access to APIs. [https://docs.aws.amazon.com/apigateway/latest/developerguide/http-api-lambda-authorizer.html](https://docs.aws.amazon.com/apigateway/latest/developerguide/http-api-lambda-authorizer.html)

• JWT authorizers use JSON web tokens to control access to APIs.

• Standard AWS IAM roles and policies offer flexible and robust access controls. You can use IAM roles and policies to control who can create and manage your APIs, as well as who can invoke them

Payload versions

1.0 REST API

2.0 HTTP API

The payload format version also determines the structure of the response that you must return from your Lambda function.

[https://github.com/broadwaylab/cognito-custom-authorizer](https://github.com/broadwaylab/cognito-custom-authorizer)

[https://github.com/SungardAS/aws-services-authorizer](https://github.com/SungardAS/aws-services-authorizer)

HttpApi:

  Type: AWS::Serverless::HttpApi

  Properties:

    StageName: $default

    Auth:

      DefaultAuthorizer: LambdaAuthorizer

      Authorizers:

        LambdaAuthorizer:

          AuthorizerPayloadFormatVersion: 2.0

          EnableSimpleResponses: true

          FunctionArn: !GetAtt LambdaAuthorizer.Arn

          Identity:

            QueryStrings:

              - auth

            Headers:

              - Authorization

            StageVariables:

              - VARIABLE

            Context:

              - authcontext

            ReauthorizeEvery: 0

[https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/sam-property-httpapi-lambdaauthorizationidentity.html](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/sam-property-httpapi-lambdaauthorizationidentity.html)

Тестирование CORS

curl -X OPTIONS [https://development.darkstore.tech/admin/](https://development.darkstore.tech/admin/)

-H "Origin: [https://development.darkstore.tech](https://development.darkstore.tech)"

-H "Access-Control-Request-Method: GET"

-v

curl -X OPTIONS [https://development.darkstore.tech/api/v1/auth/google/](https://development.darkstore.tech/api/v1/auth/google/) -H "Origin: [https://development.darkstore.tech](https://development.darkstore.tech)" -H "Access-Control-Request-Method: GET" -v