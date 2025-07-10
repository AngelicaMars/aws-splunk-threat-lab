## Create Actionable Alerts in Splunk
  # Objective:
- Detect specific security conditions in your AWS environment and trigger alerts in Splunk that a SOC (or you, the boss) can act on.

## EC2 Activity Detected Alert

**Description:** Detects when an EC2 instance is launched or terminated in AWS.
![Creating EC2 Activity Detect Alert](docs/EC2EditAlert.png)
![Activity Detected](docs/EC2activity detected.png)

**SPL:**
```spl
index=cloudtrail sourcetype=aws:cloudtrail eventSource="ec2.amazonaws.com"
(eventName="RunInstances" OR eventName="TerminateInstances")
| stats count by _time, eventName, userIdentity.arn, sourceIPAddress, requestParameters.instancesSet.items{}.instanceId

  
-- Turn It Into an Alert:
Run the SPL above

Click Save As > Alert

Set:

Title: EC2 Creation or Deletion Detected

Trigger Condition: Number of Results > 0

Trigger Actions: Log to Triggered Alerts (or email/webhook if you're feeling spicy 🌶️)

Schedule: Every 15 min, or Real-time if needed


