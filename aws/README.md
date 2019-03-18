# [AWS](#aws)

## Strategy
-----
**Strategy Used:** Switch Role

**Why?**
- Easy to setup
- Easy to manage user access
- CloudTrail audit support
- No need to create IAM Users/Access Keys

There is a CloudFormation template [here](switchrole.yaml) used to make things easier.
You can create the role through AWS Console or using the AWS CLI, but we strongly recommend the usage of CLI because it is faster and easier. 

If you really want to setup through AWS Console, [this](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/cfn-console-create-stack.html) documentation will guide you through the interface step-by-step.


## Configuration
-----

### Prerequisites
- [AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-install.html)
- [git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)


### Clone repository
```sh
git clone https://github.com/getupcloud/best-practices-access.git
cd best-practices-access/aws
```


### Deploy stack
```sh
export STACK_NAME="GetupCloudAccess"
export TRUSTED_ACCOUNT_ID="Provided AWS Account ID here"

aws cloudformation deploy \
    --stack-name ${STACK_NAME} \
    --capabilities CAPABILITY_NAMED_IAM \
    --template-file switchrole.yaml \
    --parameter-overrides TrustedAccountId=${TRUSTED_ACCOUNT_ID}
```


### Delete stack
```sh
export STACK_NAME="GetupCloudAccess"

aws cloudformation delete-stack --stack-name ${STACK_NAME}
```