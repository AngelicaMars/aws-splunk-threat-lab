## Infrastucture Overview

## AWS
- S3 Bucket: Cloudtrail and subfolder of GuardGuty
- Services: GuardDuty, CloudTrail
- IAM Role: "arn:aws:iam::...role/GuardDetector

## Splunk
- Index: 'aws_security'
- Sourcetypes: 'aws:cloudtrail', 'aws:guardduty'
- HEC:Enabled
- 
