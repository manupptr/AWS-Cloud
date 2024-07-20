# This github repository is used to showcase my work on AWS Cloud
## chapter1:AWS-CLI/cloudformation

In this chapter, i will create the labs to show the AWS resource creation through AWS-CLI and cloudformation

The below command is used to check the vpcs present in default

```
aws ec2 describe-vpcs

```
the output for the above command will be in below format
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
  ```

The below command is used to create the vpc
```
 aws ec2 create-vpc \
    --cidr-block 10.0.0.0/16 \
    --tag-specifications 'ResourceType=vpc,Tags=[{Key=Name,Value=MyVpc}]'
```

The output for the above create-vpc command will be in below format
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



