**<u>Quota</u>**: There is a certain limit in creating the resources according to the resource category
* we can raise a quote request when the resources limit is exceeded

**<u>Idempotency</u>**: The resource should be created by the code,if there is no resource exist in that category already.otherwise it shouldn't create the resource is known as idempotency in nature
* The commands that we use in shell scripts and pipeline to create the resources are not idempotent in nature
  Ex: aws ec2 create-vpc, aws ec2 run-instances etc....
* To get the idempotency while creating the resources.we can use the Iac tools like Terraform,cloudFormation,CDK etc...


### <u>cloud Formation</u>

cloud Formation is a Iac service in AWS
* it is idempotent in nature
* it a regional service
* Ex end point: cloudformation.us-east-1.amazonaws.com

cloud formation API actions Reference will be in below documentation
```
https://docs.aws.amazon.com/AWSCloudFormation/latest/APIReference/API_Operations.html
```
**Features of cloudformation:**

**1.<u>Template:</u>** It is a text file written in either json or yaml format.it contains the required infrastructure resources that we want to create

**2.<u>stack:</u>** It is a group of resources


**<u>Overview of stack Creation:</u>**
![alt text](image.png)
Template -----> CreateStack(API action) -----> Stack

* By using the CreateStack API action,We will request the cloudFormation service to create a stack for our template

<u>The following example shows the structure of a YAML-formatted template with all available sections:</u>

AWSTemplateFormatVersion: "version date"

Description:
  String

Metadata:
  template metadata

Parameters:
  set of parameters

Rules:
  set of rules

Mappings:
  set of mappings

Conditions:
  set of conditions

Transform:
  set of transforms

Resources:
  set of resources

Outputs:
  set of outputs


**<u>From the above CFT format.The main necessary sections are</u>**

<u>AWSTemplateFormatVersion:</u> "version date"

<u>Description:</u> about template uses

<u>Resources:</u> resources shoud we need to create 


**3.<u>Change-Sets:</u>** It previews the changes between old template and new template that what resources we are going to change or update in our infrastructure
> it is very useful to find the errors before deploying the template into the production
> we can rollback to the previous changes easily by updating the original template 

**4.<u>Drift detection:</u>** it can indentify the changes of our AWS resources which differs from the resources specified in our cloudformation template

**5.<u>Stack Sets:</u>**