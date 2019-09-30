# AWS Engineering Learning Series - EKS

## Labs

- [Lab 1 - Building a Docker Image](./labs/01-docker)
- [Lab 2 - Introduction to Pods](./labs/02-pods)
- [Lab 3 - Playing around with our Pod](./labs/03-more-pods)
- [Lab 4 - Labeling our Pods](./labs/04-labels)
- [Lab 5 - Deployments](./labs/05-deployments)
- [Lab 6 - Services](./labs/06-services)


## Launching your Lab Environment

Follow the below steps to get setup with a Cloud9 environment and create your Cluster.

## 1. Launch a Cloud9 Environment

This will be where you'll be performing the labs throughout the sessions.

Click the button to begin creating a CloudFormation stack for the region you are assigned.

Preferably right click an open it in a new tab.

| Region          | CloudFormation     |
| --------------- |:------------------:|
| eu-west-1 (Ireland)       | [![Launch Stack](https://s3.amazonaws.com/cloudformation-examples/cloudformation-launch-stack.png)](https://console.aws.amazon.com/cloudformation/home?region=eu-west-1#/stacks/create/review?stackName=cloud9&templateURL=https://eks2019.s3-ap-southeast-2.amazonaws.com/cloud9-template.yml) |
| eu-north-1 (Stockholm)       | [![Launch Stack](https://s3.amazonaws.com/cloudformation-examples/cloudformation-launch-stack.png)](https://console.aws.amazon.com/cloudformation/home?region=eu-north-1#/stacks/create/review?stackName=cloud9&templateURL=https://eks2019.s3-ap-southeast-2.amazonaws.com/cloud9-template.yml) |
| us-east-1 (N. Virginia)       | [![Launch Stack](https://s3.amazonaws.com/cloudformation-examples/cloudformation-launch-stack.png)](https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/create/review?stackName=cloud9&templateURL=https://eks2019.s3-ap-southeast-2.amazonaws.com/cloud9-template.yml) |
| us-west-2 (Oregon)       | [![Launch Stack](https://s3.amazonaws.com/cloudformation-examples/cloudformation-launch-stack.png)](https://console.aws.amazon.com/cloudformation/home?region=us-west-2#/stacks/create/review?stackName=cloud9&templateURL=https://eks2019.s3-ap-southeast-2.amazonaws.com/cloud9-template.yml) |
| ap-southeast-1 (Singapore)  | [![Launch Stack](https://s3.amazonaws.com/cloudformation-examples/cloudformation-launch-stack.png)](https://console.aws.amazon.com/cloudformation/home?region=ap-southeast-1#/stacks/create/review?stackName=cloud9&templateURL=https://eks2019.s3-ap-southeast-2.amazonaws.com/cloud9-template.yml) |

Just before clicking "Create stack" button, please tick "I acknowledge that AWS CloudFormation might create IAM resources."

If you get stuck, the CloudFormation template is available [here](https://eks2019.s3-ap-southeast-2.amazonaws.com/cloud9-template.yml) and also in this [repo](./cloudformation/cloud9-template.yaml).

## 2. Attach the IAM Role to the Cloud9 instance

This will allow your cloud9 environment access to perform the actions needed for the sessions.

This can be done in the EC2 console, navigate to your EC2 instances, or click the link below:

Preferably right click an open it in a new tab.

| Region          | EC2     |
| --------------- |:------------------:|
| eu-west-1 (Ireland)       | [Console link](https://eu-west-1.console.aws.amazon.com/ec2/v2/home?region=eu-west-1#Instances:tag:Name=cloud9;sort=instanceState) |
| eu-north-1 (Stockholm)       | [Console link](https://eu-north-1.console.aws.amazon.com/ec2/v2/home?region=eu-north-1#Instances:tag:Name=cloud9;sort=instanceState) |
| us-east-1 (N. Virginia)      | [Console link](https://us-east-1.console.aws.amazon.com/ec2/v2/home?region=us-east-1#Instances:tag:Name=cloud9;sort=instanceState) |
| us-west-2 (Oregon)       | [Console link](https://us-west-2.console.aws.amazon.com/ec2/v2/home?region=us-west-2#Instances:tag:Name=cloud9;sort=instanceState) |
| ap-southeast-1 (Singapore)  | [Console link](https://ap-southeast-1.console.aws.amazon.com/ec2/v2/home?region=ap-southeast-1#Instances:tag:Name=cloud9;sort=instanceState) |

 * Select the Cloud9 instance
 * Click Actions > Instance Settings > Attach/Replace IAM Role
 * Filter the roles, searching for "cloud9"
 * Click Apply once the role is selected

## 3. Access your Cloud9 environment

This can be done in the Cloud9 console, navigate to Cloud9 or click the link below:

Preferably right click an open it in a new tab.


| Region          | EC2     |
| --------------- |:------------------:|
| eu-west-1 (Ireland)       | [Console link](https://eu-west-1.console.aws.amazon.com/cloud9/home?region=eu-west-1) |
| eu-north-1 (Stockholm)       | [Console link](https://eu-north-1.console.aws.amazon.com/cloud9/home?region=eu-north-1) |
| us-east-1 (N. Virginia)      | [Console link](https://us-east-1.console.aws.amazon.com/cloud9/home?region=us-east-1) |
| us-west-2 (Oregon)        | [Console link](https://us-west-2.console.aws.amazon.com/cloud9/home?region=us-west-2) |
| ap-southeast-1 (Singapore)  | [Console link](https://ap-southeast-1.console.aws.amazon.com/cloud9/home?region=ap-southeast-1) |

 * Click Open IDE

## 4. Setup the Cloud9 environment

The environment will be our workstation for the sessions, there are a few steps needed to get it setup

* From within the Cloud9 environment perform the below steps:

  * Click on AWS Cloud9 (top left) > Preferences
  * Click on AWS SETTINGS > Credentials
  * Turn off 'AWS managed temporary credentials'

* Change to a dark theme if you prefer:

  * View > Themes > UI Themes > Classic Dark

### Run the below commands in the Cloud9 terminal

#### Clone the repository

```bash
$ git clone https://github.com/aws-els-cpt/eks.git
```

#### Run the bootstrap script

The script installs and configures the necessary pre-requisites

```bash
$ eks/scripts/bootstrap.sh
```

Confirm the IAM role is as expected

---

# Scaling Down your Worker Nodes

Your voucher should cover the costs for the full period of the learning series but if, between sessions, you want to
prevent extra costs you can scale down your clusters using `eksctl` as follows:

```bash
$ eksctl get clusters
$ eksctl get nodegroup --cluster eks
$ eksctl scale nodegroup --cluster=eks --nodes=0 ng-xxxxxxx
```

_Note: As of 2019-09-30 there is a bug with the above command [github/issues/809](https://github.com/weaveworks/eksctl/issues/809)_that does not update the min value of the auto scaling group. To get around this scale the nodes to 0, then to 1 and then to 0 again (using the last command above)._

# Cleanup (Only do this after the end of all three sessions)

After the end of all three sessions, follow the below steps to delete the EKS cluster and Cloud9 environment

## Delete the EKS cluster

This can be done in the Cloud9 console, navigate to Cloud9, or click the link below:

| Region          | EC2     |
| --------------- |:------------------:|
| eu-west-1 (Ireland)       | [Console link](https://eu-west-1.console.aws.amazon.com/cloud9/home?region=eu-west-1) |
| eu-north-1 (Stockholm)       | [Console link](https://eu-north-1.console.aws.amazon.com/cloud9/home?region=eu-north-1) |
| us-east-1 (N. Virginia)      | [Console link](https://us-east-1.console.aws.amazon.com/cloud9/home?region=us-east-1) |
| us-west-2 (Oregon)       | [Console link](https://us-west-2.console.aws.amazon.com/cloud9/home?region=us-west-2) |
| ap-southeast-1 (Singapore)  | [Console link](https://ap-southeast-1.console.aws.amazon.com/cloud9/home?region=ap-southeast-1) |

* Delete the EKS cluster from the Cloud9 terminal with the below command:

```bash
$ eksctl delete cluster eks
```

## Delete the Cloud9 CloudFormation stack

This can be done in the CloudFormation console, navigate to CloudFormation, or click the link below:

| Region          | EC2     |
| --------------- |:------------------:|
| eu-west-1 (Ireland)       | [Console link](https://eu-west-1.console.aws.amazon.com/cloudformation/home?region=eu-west-1) |
| eu-north-1 (Stockholm)       | [Console link](https://eu-west-1.console.aws.amazon.com/cloudformation/home?region=eu-north-1) |
| us-east-1 (N. Virginia)      | [Console link](https://us-east-1.console.aws.amazon.com/cloudformation/home?region=us-east-1) |
| us-west-2 (Oregon)       | [Console link](https://us-west-2.console.aws.amazon.com/cloudformation/home?region=us-west-2) |
| ap-southeast-1 (Singapore)  | [Console link](https://ap-southeast-1.console.aws.amazon.com/cloudformation/home?region=ap-southeast-1) |

There may be a number of stacks, select the stack named "cloud9", and click the "Delete" button
