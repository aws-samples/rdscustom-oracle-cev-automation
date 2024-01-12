# Automate Custom Engine Version ( CEV) creation with AWS CloudFormation Template in Amazon RDS Custom for Oracle

## Solution Overview 

We propose a solution to create a CEV by using a CFN template. We provide a CFN template — you just need to download the template and follow the deployment steps to build the CEV. Currently, the template is developed to create the latest 19c version CEV.

This solution uses the following services and resources:

* Amazon Elastic Compute Cloud (Amazon EC2): Amazon EC2 provides scalable compute capacity in the AWS Cloud. In this solution, we use Amazon EC2 to perform pre-work for creating the CEV.
* Amazon S3: Amazon S3 is an object storage service that offers industry-leading scalability, data availability, security, and performance. In this solution, we use Amazon S3 to store downloaded Oracle patches.
* AWS CloudFormation:  AWS CloudFormation is a service that helps you model and set up your AWS resources. You create a template that describes all the AWS resources that you want (e.g., Amazon EC2 instances, IAM roles, or CEV). AWS CloudFormation takes care of provisioning and configuring those resources for you. In this solution, we deploy all the resources (i.e., EC2 instances, IAM role, CEV) using an AWS CloudFormation script.
* AWS Identity and Access Management (AWS IAM): AWS IAM is a web service that helps you securely control access to AWS resources. With AWS IAM, you can centrally manage permissions that control which AWS resources users can access.
AWS CloudFormation template

## High Level Steps

In this solution, we provide an AWS CloudFormation template that takes minimum input parameters from the user and performs the following high-level tasks:
1.	Read key input parameters for provisioning like Amazon EC2 instance, EC2 instance type, OTN Credentials and more.
2.	Provision an Amazon EC2 instance that downloads the patches and validate the checksum.
3.	Create an AWS IAM role to interact with AWS Services deployed in this solution.
4.	Create a CEV for Amazon RDS Custom for Oracle.

## Prerequisites

This solution requires the following prerequisites before you run it:

* An AWS account with the AWS IAM permissions to create and manage keys, Amazon RDS, Amazon Elastic Compute Cloud (Amazon EC2), Amazon Simple Storage Service (Amazon S3), AWS CloudFormation, and Virtual Private Cloud (VPC)-related resources.
* An Amazon S3 bucket is created.
* An Oracle support contract and license to access edelivery.oracle.com and download installers and patches.
* A VPC with either a Public Subnet or a private subnet access to internet to download the oracle patches and necessary packages for this solution.
* An AWS Region where Amazon RDS Custom is available.

## List of input parameters to be ready with before deploying the stack

* S3Bucket - This is the existing Amazon S3 bucket to store the downloaded Oracle patches.
* S3Prefix – Yes/No – Does S3 prefix present in your S3 bucket?
* S3BucketPrefix – Enter the S3 bucket prefix for folder structure; if not, leave it blank.
* KMSKeyID - This is the existing KMSKeyID used to encrypt.
* EngineType - RDS custom for Oracle support both NON-CDB (i.e., custom-oracle-ee) and CDB (i.e., custom-oracle-ee-cdb).
* EngineVersion - The name of your custom engine version (CEV).
* LatestAmiId – Image ID is from the Parameter Store of SSM. Leave it as-is.
* EC2SubnetID - Existing subnet where the Amazon EC2 instance is created.
* EC2SecurityGroup - Existing security group for Amazon EC2 Instance.
* DBVersion - Provide the DBversion to download the oracle patches and upload them to the Amazon S3 bucket.
* OracleAccountUser - Oracle Account Username to download the patches.
* OracleAccountPassword - Oracle Account Password to download the patches.

## Deploying the solution

You can use this solution through the AWS Management Console or run it via the AWS Command Line Interface (AWS CLI). This solution assumes that you’re familiar with the deployment of the AWS CloudFormation template; however, if you require instructions on how to run a CloudFormation template on AWS, then follow the Get started guide.

To deploy this solution in your account, complete the following steps:

1.	Download the YAML file from [GitHub repository](https://github.com/aws-samples/rdscustom-oracle-cev-automation) to your local machine.
2.	Sign in to the AWS Management Console and open the [CloudFormation console](https://console.aws.amazon.com/cloudformation). Choose the same AWS Region where your RDS Custom primary DB instance resides.
3.  Choose Create Stack. 
4.  In the Create Stack page, do the following:
     a. Choose Template is ready.
     b. In Upload a template file, choose the YAML file that you downloaded in Step 1.
     c. Choose Next.
5.  In Specify Stack Details, enter the following details, and then choose Next:

* For Stack name, enter a descriptive name. The following example uses the name format RDSCustom-Oracle-CEV-Automation

The following screenshot summarizes the parameters for our stack creation.

![Arch](/Images/CFN-parameters.png)

When the stack creation is complete, navigate to the stack and choose the Resources tab to review all the resources that were created as part of this CloudFormation template.

![Arch](/Images/CFN-deployed-resources.png)

Note: Deployment of this AWS CloudFormation template can take up to three hours to create a CEV in this solution. The CEV itself takes at least two hours.

## Cleaning up your resources

When you clean up, you can either delete individual resources or the entire stack. We recommend that you delete resources individually as needed.

### Deleting the full stack

If you choose to delete the full stack at once, be aware that the deletion policy for CloudFormation retains the EC2 Instance and RDS Custom role. Turn off this setting at your own risk.

## Contributors 
- Sharath Chandra Kampili
- Pavan Vukkisila

## Security

See [CONTRIBUTING](CONTRIBUTING.md#security-issue-notifications) for more information.

## License

This library is licensed under the MIT-0 License. See the LICENSE file.


