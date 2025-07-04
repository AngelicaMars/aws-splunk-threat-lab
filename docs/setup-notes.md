#  Setup Notes

## 1. AWS IAM User Creation
  -Created IAM user with programmatic access 'Splunk-reader'
  -Attached 'Security Audit' and custom S# policy

## 2. S3 Bucket
  - Confirmed Cloudtrail is writing to: 'S3://aws-cloudtrail-logs-536285029732-1c51c043/GuardDuty'

## 3. Splunk Configuration
  - Installed AWS Add-on
  - Configured inputs:
        CloudTrail > Index=cloudtrail sourcetype= aws:cloudtrail
        GuardDuty >  Index=aws_security sourcetype= aws:guardduty
  - Verified data in index: "cloudtrail" & "guardduty"
 
## 4. GuardDuty Integration
  - Created IAM role: 'GuardDutyDetector'
  - Granted read permissions for GuardDuty
  - confirmed data in 'aws:guardduty' sourcetype

## 5. Verification
  - Search Splunk: 'index=cloudtrail sourcetype=aws:cloudtrail | stats count'
  - Search Findings: 'index=aws_security sourcetype=aws:guardduty'  

## 6. Alerts & Dashboards
  - Example SPL for failed  logins
  - Example dashboard layout
