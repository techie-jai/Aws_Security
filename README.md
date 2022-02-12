**# Aws_Security
****This repo includes AWS Security related scripts and automations
**
**Codes/Scripts and their uses:**

1. **S3_BUCKET_VERSIONING_ENABLED.template::** Upload this script to CFN and let it create resources. After creating resources, this will automatically enable versioning in all the buckets which were marked as non-compliant by the config rule **	s3-bucket-versioning-enabled**.

2. **config-rule-and-auto-remediation-s3-encryption** : This scirpt first creates a new rule in config s**3-bucket-server-side-encryption-enabled** and then proceeds to remediate all the bucket found by this rule as non compliant since they were not encrypted. After the lunch of the template on CFN, first check the deployed rule by this temp on config, you will see all the s3 buckets that are non-compliant, then click on reevaluate and slowly slowly all the buckets will start to become compliant since at backend a lambda was deployed by template to do this.

3. **EC2_INSTANCE_NO_PUBLIC_IP** When you deploy this template on CFN, AWS will create a config rule "ec2-instance-no-public-ip" and then deploy remediation action which will stop all the instances with public IPs

4 & 5. **APPROVED_AMIS_BY_ID & Tags**: These templates will deploy rules which takes AMI ID or Tags as input parameter and further uses SSM documents to turn of the non compliant EC2 instances as a remediation.

6. **automate-cloudtrail-monitoring-alert**: After deploying this template, if someone stops CloudTrail logging, you'll get an SNS notification about the same and lambda function will restart the logging.

7. security-automation-remediate-unintended-iam-access-master: This will create a lambda function which will be invoked by a even bridge which gets its events from cloudtrail. So if somebody does something like creating a user, the event bridge will match it and invoke the lambda which will further attach an deny all policy to that IAM user if the user was not found to be part of an IAM admin group. BUt please remember to edit the event bridge since right now it will invoke lamnbda if any iam api call is made. Check this: https://github.com/miztiik/security-automation-remediate-unintended-iam-access
**

**8 .remove-unused-security-groups.template**: This template deploys a lambda which is triggered weekly. Once triggered, it will first create a list of all the secruity grioups in the account, then it will exclude the once which are currently attached to an Ec2 and then removes the once that are left ie that are not attached to any ec2. This then returns a string with all the deleted security group list. THis gets triggered by a cloudwatch event weekly.

**10. security-automation-remediate-weak-s3-policy.template.json** : This template will deploy lambda which triggers everytime a policy is changed. It then checks the previous policy and reverts back to the original policy. uses config, event bridge etc. https://github.com/miztiik/security-automation-remediate-weak-s3-policy

