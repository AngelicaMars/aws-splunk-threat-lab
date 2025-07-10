## Create Actionable Alerts in Splunk

###  Objective:
Detect specific security conditions in your AWS environment and trigger alerts in Splunk that a SOC (or you — the boss 😎) can act on.


### EC2 Activity Detected Alert

**Description:**  
Detects when an EC2 instance is launched or terminated in AWS. Useful for identifying unauthorized provisioning or unexpected deletions.


### SPL Query:

index=cloudtrail sourcetype=aws:cloudtrail eventSource="ec2.amazonaws.com"
(eventName="RunInstances" OR eventName="TerminateInstances")
| stats count by _time, eventName, userIdentity.arn, sourceIPAddress, requestParameters.instancesSet.items{}.instanceId

## Steps to Create the Alert:
Run the SPL above in Splunk Search

Click Save As > Alert

Configure the alert settings:

Title: EC2 Creation or Deletion Detected

Trigger Condition: Number of Results > 0

Schedule: Every 15 minutes (or Real-time if needed)

Trigger Actions: Log to Triggered Alerts

## Screenshot Examples

![EC2 Activity Detected](docs/EC2EditAlert.png)
