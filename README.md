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
For example if we want to list the s3 buckets through github actions,we have to add the below github action under the steps section in the above code
```
-run: aws s3 ls
```
