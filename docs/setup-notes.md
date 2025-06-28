#  Setup Notes

## 1. AWS IAM User Creation
-Created IAM user with programmatic access
Attached 'Security Audit' and custom S# policy

## 2. S3 Bucket
- Confirmed Cloudtrail is writing to: 'S3://aws-cloudtrail-logs-....'

## 3. Splunk Configuration
  - Installed AWS Add-on
  - Configured input with sourcetype: 'aws:s3'
  - Verified data in index: "cloudtrail"
 
## 4. GuardDuty Integration
  - Created IAM role: 'SplunkGuardDutyRole'
  - Granted read permissions for GuardDuty
  - confirmed data in 'aws:guardduty' sourcetype 
