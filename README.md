# This github repository is used to showcase my work on AWS Cloud
## chapter1:AWS-CLI/cloudformation

In this chapter, i will create the labs to show the AWS resource creation through AWS-CLI and cloudformation


1.The below command is used to create the vpc:
```
 aws ec2 create-vpc \
    --cidr-block 10.0.0.0/16 \
    --tag-specifications 'ResourceType=vpc,Tags=[{Key=Name,Value=MyVpc}]'
```

The output for the above create-vpc command will be in below format:
```
Vpc:
  CidrBlock: 10.0.0.0/16
  CidrBlockAssociationSet:
  - AssociationId: vpc-cidr-assoc-030a4dde2986d1e09
    CidrBlock: 10.0.0.0/16
    CidrBlockState:
      State: associated
  DhcpOptionsId: dopt-0b79acfe1af8ee90b
  InstanceTenancy: default
  Ipv6CidrBlockAssociationSet: []
  IsDefault: false
  OwnerId: '1234567890'
    State: available
    Tags:
    - Key: Name
      Value: MyVpc
  VpcId: vpc-009ec6504e1eaceb3
```

To check our vpc is created or not ,the below command is used .it lists all the vpcs present in our aws:
```
Vpcs:
- CidrBlock: 172.31.0.0/16
  CidrBlockAssociationSet:
  - AssociationId: vpc-cidr-assoc-0200952eefdfe08fa
    CidrBlock: 172.31.0.0/16
    CidrBlockState:
      State: associated
  DhcpOptionsId: dopt-0b79acfe1af8ee90b
  InstanceTenancy: default
  IsDefault: true
  OwnerId: '1234567890'
  State: available
  Tags:
  - Key: Name
    Value: by default
  VpcId: vpc-04c491e2119b1f9ab
- CidrBlock: 10.0.0.0/16
  CidrBlockAssociationSet:
  - AssociationId: vpc-cidr-assoc-030a4dde2986d1e09
    CidrBlock: 10.0.0.0/16
    CidrBlockState:
      State: associated
  DhcpOptionsId: dopt-0b79acfe1af8ee90b
  InstanceTenancy: default
  IsDefault: false
  OwnerId: '1234567890'
  State: available
  Tags:
  - Key: Name
    Value: MyVpc
  VpcId: vpc-009ec6504e1eaceb3
```

To check the status of certain vpc with that vpc-id, the below command is used:

```
aws ec2 describe-vpcs --vpc-id <vpc-id>
```

The output for the vpc status checked with a certain vpc-id (json format)

```
{
    "Vpcs": [
        {
            "CidrBlock": "10.0.0.0/16",
            "DhcpOptionsId": "dopt-0b79acfe1af8ee90b",
            "State": "available",
            "VpcId": "vpc-009ec6504e1eaceb3",
            "OwnerId": "1234567890",
            "InstanceTenancy": "default",
            "CidrBlockAssociationSet": [
                {
                    "AssociationId": "vpc-cidr-assoc-030a4dde2986d1e09",
                    "CidrBlock": "10.0.0.0/16",
                    "CidrBlockState": {
                        "State": "associated"
                    }
                }
            ],
            "IsDefault": false,
            "Tags": [
                {
                    "Key": "Name",
                    "Value": "MyVpc"
                }
            ]
        }
    ]
}
```

2.To delete vpc ,The below command is used

```
aws ec2 delete-vpc --vpc-id <vpc-id>
```

### Github actions and workflows

create a .github/workflows directory under the root directory (AWS Cloud)
```
mkdir -p .github/actions
```

create a file called 'github-actions-demo-yml' in visual studio and configure the file with the below content
```
name: GitHub Actions Demo
run-name: ${{ github.actor }} is testing out GitHub Actions 🚀
on: [push]
jobs:
  Explore-GitHub-Actions:
    runs-on: ubuntu-latest
    steps:
      - run: echo "🎉 The job was automatically triggered by a ${{ github.event_name }} event."
      - run: echo "🐧 This job is now running on a ${{ runner.os }} server hosted by GitHub!"
      - run: echo "🔎 The name of your branch is ${{ github.ref }} and your repository is ${{ github.repository }}."
      - name: Check out repository code
        uses: actions/checkout@v4
      - run: echo "💡 The ${{ github.repository }} repository has been cloned to the runner."
      - run: echo "🖥️ The workflow is now ready to test your code on the runner."
      - name: List files in the repository
        run: |
          ls ${{ github.workspace }}
      - run: echo "🍏 This job's status is ${{ job.status }}."
```

Note:Before performing the below actions we need to install aws-cli and configure the aws-cli with access key and secret access key in our local machine

How to install aws-cli according to our machine type like linux,windows and mac, follow the steps provided in the below link
```
https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html
```

To confirm the aws-cli is installed or not ,enter the below command in your local terminal
```
aws --version
```

If the aws-cli is installed ,the output will be looks like below

```
aws-cli/2.17.2 Python/3.11.8 Windows/10 exe/AMD64
```

To configure the aws-cli ,use the below command and enter the access key,secret access key,region and output format
```
aws configure
```

we can modify the github actions in our code
For example if we want to list the s3 buckets through github actions,we have to add the below github actions under the steps section in the github-actions-demo-yml file
```
- uses: aws-actions/configure-aws-credentials@v2
        with:
          aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
          aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          aws-region: us-east-1
```

To create vpc and describe vpcs through github actions using aws-cli
modify the below actions under the steps section in github-actions-demo-yml
```
- run: aws ec2 create-vpc --cidr-block 10.0.0.0/16 --tag-specifications 'ResourceType=vpc,Tags=[{Key=Name,Value=MyVpc}]'
- run: aws ec2 descibe-vpcs
```

When ever we push the github-actions-demo-yml file to our github ,it creates the vpc and describe the vpcs in our aws account with the below action
```
on: [push]
```
Otherwiswe we can run the github action manually when ever we need by modifing the github-actions-demo-yml file by replacing the above action with below one
```
on:
  workflow_dispatch:
    inputs:
      my_input:
        description: my input description
        required: false
        type: string
```
when we run the workflow manually in github actions, it creates the resources for each run. 
For example if we run the workflow for 3 times,it creates 3 vpcs in our slected region


### VPC creation through Cloudformation template

Creating a YAML Template to create a vpc
```
AWSTemplateFormatVersion: "2010-09-09"
Description:
  This template creates a vpc in selected region
  author:Manu
Resources:
  MyCfnVPC:
    Type: AWS::EC2::VPC
    Properties:
      CidrBlock: 10.0.0.0/16
      Tags:
      - key: Name
        value: MyTestVPC
      - key: environment
        value: Prod
      - key: department
        value: DevOps
```
### creating the stack through AWS-CLI

To create the stack for creating vpc by using the same above CFT.The below command is used
```
aws cloudformation create-stack --stack-name mycfnstack --template-body file://createVPC.yaml
```
The stack will be created and the output will be look like below
```
{
    "StackId": "arn:aws:cloudformation:us-east-1:381492281943:stack/mycfnstack/c19f4880-485b-11ef-9e10-1227813a2bf9"
}
```

To describe the stack events.The below command is used
```
aws cloudformation describe-stack-events --stack-name mycfnstack
```
The output for the above command will be look like below
```
{
    "StackEvents": [
        {
            "StackId": "arn:aws:cloudformation:us-east-1:381492281943:stack/mycfnstack/c19f4880-485b-11ef-9e10-1227813a2bf9",
            "EventId": "caf67cf0-485b-11ef-9f40-0eb3646ef60b",
            "StackName": "mycfnstack",
            "LogicalResourceId": "mycfnstack",
            "PhysicalResourceId": "arn:aws:cloudformation:us-east-1:381492281943:stack/mycfnstack/c19f4880-485b-11ef-9e10-1227813a2bf9",
            "ResourceType": "AWS::CloudFormation::Stack",
            "Timestamp": "2024-07-22T18:54:15.602000+00:00",
            "ResourceStatus": "CREATE_COMPLETE"
        },
        {
            "StackId": "arn:aws:cloudformation:us-east-1:381492281943:stack/mycfnstack/c19f4880-485b-11ef-9e10-1227813a2bf9",
            "EventId": "MyCfnVPC-CREATE_COMPLETE-2024-07-22T18:54:14.922Z",
            "StackName": "mycfnstack",
            "LogicalResourceId": "MyCfnVPC",
            "PhysicalResourceId": "vpc-016581e2c3058c468",
            "ResourceType": "AWS::EC2::VPC",
            "Timestamp": "2024-07-22T18:54:14.922000+00:00",
            "ResourceStatus": "CREATE_COMPLETE",
            "ResourceProperties": "{\"CidrBlock\":\"192.168.0.0/16\",\"Tags\":[{\"Value\":\"MyTestVPC\",\"Key\":\"Name\"},{\"Value\":\"Prod\",\"Key\":\"environment\"},{\"Value\":\"DevOps\",\"Key\":\"department\"}]}"
        },
        {
            "StackId": "arn:aws:cloudformation:us-east-1:381492281943:stack/mycfnstack/c19f4880-485b-11ef-9e10-1227813a2bf9",
            "EventId": "MyCfnVPC-CREATE_IN_PROGRESS-2024-07-22T18:54:03.896Z",
            "StackName": "mycfnstack",
            "LogicalResourceId": "MyCfnVPC",
            "PhysicalResourceId": "vpc-016581e2c3058c468",
            "ResourceType": "AWS::EC2::VPC",
            "Timestamp": "2024-07-22T18:54:03.896000+00:00",
            "ResourceStatus": "CREATE_IN_PROGRESS",
            "ResourceStatusReason": "Resource creation Initiated",
            "ResourceProperties": "{\"CidrBlock\":\"192.168.0.0/16\",\"Tags\":[{\"Value\":\"MyTestVPC\",\"Key\":\"Name\"},{\"Value\":\"Prod\",\"Key\":\"environment\"},{\"Value\":\"DevOps\",\"Key\":\"department\"}]}"
        },
        {
            "StackId": "arn:aws:cloudformation:us-east-1:381492281943:stack/mycfnstack/c19f4880-485b-11ef-9e10-1227813a2bf9",
            "EventId": "MyCfnVPC-CREATE_IN_PROGRESS-2024-07-22T18:54:02.516Z",
            "StackName": "mycfnstack",
            "LogicalResourceId": "MyCfnVPC",
            "PhysicalResourceId": "",
            "ResourceType": "AWS::EC2::VPC",
            "Timestamp": "2024-07-22T18:54:02.516000+00:00",
            "ResourceStatus": "CREATE_IN_PROGRESS",
            "ResourceProperties": "{\"CidrBlock\":\"192.168.0.0/16\",\"Tags\":[{\"Value\":\"MyTestVPC\",\"Key\":\"Name\"},{\"Value\":\"Prod\",\"Key\":\"environment\"},{\"Value\":\"DevOps\",\"Key\":\"department\"}]}"
        },
        {
            "StackId": "arn:aws:cloudformation:us-east-1:381492281943:stack/mycfnstack/c19f4880-485b-11ef-9e10-1227813a2bf9",
            "EventId": "c1a19270-485b-11ef-9e10-1227813a2bf9",
            "StackName": "mycfnstack",
            "LogicalResourceId": "mycfnstack",
            "PhysicalResourceId": "arn:aws:cloudformation:us-east-1:381492281943:stack/mycfnstack/c19f4880-485b-11ef-9e10-1227813a2bf9",
            "ResourceType": "AWS::CloudFormation::Stack",
            "Timestamp": "2024-07-22T18:54:00+00:00",
            "ResourceStatus": "CREATE_IN_PROGRESS",
            "ResourceStatusReason": "User Initiated"
        }
    ]
}
```

To list the stacks in cloudformation.The below command is used
```
aws cloudformation describe-stacks
```

To delete the stack by using the below command.Then our stack gets deleted
```
aws cloudformation delete-stack --stack-name mycfnstack
```

### creating the stack in cloudformation through github actions

CReate a create-vpc-cft-github.yaml in the .github/workflows directory and place the below content in that file
````
name: cfn vpc github action
on:
  workflow_dispatch:
    inputs:
      my_input:
        description: my input description
        required: false
        type: string
jobs:
  cft-vpc-github-action:
    runs-on: ubuntu-latest
    steps:
      - name: Check out repository code
        uses: actions/checkout@v4
      - uses: aws-actions/configure-aws-credentials@v2
        with:
          aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
          aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          aws-region: us-east-1
      - run: aws cloudformation create-stack --stack-name mycfnstack --template-body=file://cloudformation/createVPC.yaml
      - run: sleep 20
      - run: aws cloudformation describe-stack-events --stack-name mycfnstack
```