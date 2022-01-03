**# Aws_Security
****This repo includes AWS Security related scripts and automations
**
**Codes/Scripts and their uses:**

1. **S3_BUCKET_VERSIONING_ENABLED.template::** Upload this script to CFN and let it create resources. After creating resources, this will automatically enable versioning in all the buckets which were marked as non-compliant by the config rule **	s3-bucket-versioning-enabled**.

2. **config-rule-and-auto-remediation-s3-encryption** : This scirpt first creates a new rule in config s**3-bucket-server-side-encryption-enabled** and then proceeds to remediate all the bucket found by this rule as non compliant since they were not encrypted. After the lunch of the template on CFN, first check the deployed rule by this temp on config, you will see all the s3 buckets that are non-compliant, then click on reevaluate and slowly slowly all the buckets will start to become compliant since at backend a lambda was deployed by template to do this.

3. When you deploy this template on CFN, AWS will create a config rule "ec2-instance-no-public-ip" and then deploy remediation action which will stop all the instances with public IPs
