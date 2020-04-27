# [AWS](#aws)

- [Strategy](README.md#strategy)
- [Configuration](README.md#configuration)
- [Cloudformation Template](README.md#cloudformation-template)


## [Strategy](#strategy)

**Strategy Used:** Switch Role

**Why?**

- Easy to setup
- Easy to manage user access
- CloudTrail audit support
- No need to create IAM Users/Access Keys

There is a CloudFormation template [here](switchrole.yaml) used to make things easier.
You can create the role through AWS Console or using the AWS CLI, but we strongly recommend the usage of CLI because it is faster and easier. 

If you really want to setup through AWS Console, [this](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/cfn-console-create-stack.html) documentation will guide you through the interface step-by-step.

## [Configuration](#configuration)

### Prerequisites

- [AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-install.html)
- [git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)

### Clone repository

Clone the repository and change your current folder:

```sh
git clone https://github.com/getupcloud/best-practices-access.git
cd best-practices-access/aws
```

### Deploy stack

Create/Update the Cloudformation stack:

```sh
export TRUSTED_ACCOUNT_ID="975877104335"
export STACK_NAME="GetupCloudAccess"

aws cloudformation deploy \
    --stack-name ${STACK_NAME} \
    --region us-east-1 \
    --capabilities CAPABILITY_NAMED_IAM \
    --template-file switchrole.yaml \
    --parameter-overrides TrustedAccountId=${TRUSTED_ACCOUNT_ID}
```

- *You can override Cloudformation's template default variables by setting extra parameters ```--parameter-overrides ParamA=ValueA ParamB=ValueB```.*

### Get stack output

Print the Ouput Variables from the previously created stack:

```sh
export STACK_NAME="GetupCloudAccess"

aws cloudformation describe-stacks \
    --stack-name ${STACK_NAME} \
    --region us-east-1 \
    --query 'Stacks[0].Outputs' \
    --output text
```

- *You'll send us the stack output, as we need this information to finish the access setup.*

### Delete stack

Delete the created Cloudformation stack:

```sh
export STACK_NAME="GetupCloudAccess"

aws cloudformation delete-stack \
    --stack-name ${STACK_NAME} \
    --region us-east-1
```

- *WARNING: Deleting the stack will delete any resources created with it (roles & policies), consequently disabling all accesses as well.*

## [Cloudformation Template](#cloudformation-template)

## Parameters

Allow access from the US account (Mandatory)

| Parameter            | Type         | Recommended                | Example          |
|----------------------|--------------|----------------------------|------------------|
| TrustedAccountId     | String       | Our AWS Account ID         | 975877104335     |
| UseAdminPolicy       | String       | The default value          | true             |
| RoleName             | String       | The default value          | getupcloud       |

Allow access from the BR account (Optional)

| Parameter            | Type         | Recommended                | Example          |
|----------------------|--------------|----------------------------|------------------|
| TrustedAccountId     | String       | Our AWS Account ID         | 048671028587     |
| UseAdminPolicy       | String       | The default value          | true             |
| RoleName             | String       | The default value          | getupcloud       |
