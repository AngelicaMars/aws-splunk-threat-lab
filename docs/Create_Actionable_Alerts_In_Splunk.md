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
![Real time Alert example](docs/EC2activitydetected.png)


## GuardDuty Visual Dashboard - SPL Summary

1. GuardDuty Alert Table

Shows a detailed list of alerts with filtering for severity and application context.

index=aws_security sourcetype=aws:guardduty
| spath
| search severity="$severity_filter$"
| eval App="$app_filter$"
| table _time, title, severity, type, region, resource.resourceType
| rename title as "Alert Title", severity as Severity, type as "Alert Type", region as Region, resource.resourceType as "Resource Type"

2. Severity Distribution
 Displays the count of alerts by severity level.
 
index=aws_security sourcetype=aws:guardduty
| spath
| search severity="$severity_filter$"
| stats count by severity
| rename severity as Severity, count as Count


3. Alert Severity Over Time
Tracks alert volume trends by severity on an hourly basis.

index=aws_security sourcetype=aws:guardduty
| spath
| search severity="$severity_filter$"
| timechart count by severity span=1h

- Purpose: Shows how alert severity levels trend over time (hourly view)
- Visual Type: Line chart or area chart
- Insight: Spot spikes in severity or times of increased attack activity


##  How to Create a Real-Time Studio Dashboard for AWS GuardDuty Alerts

This guide walks you through building a real-time visual dashboard in **Splunk Dashboard Studio** using GuardDuty data from your AWS environment.

### Prerequisites:
- Splunk Cloud or Enterprise with Dashboard Studio enabled
- GuardDuty data ingested into Splunk (sourcetype=aws:guardduty > S3 > Splunk input)
- An index like aws_security with searchable GuardDuty events


###  Step-by-Step: Build the Real-Time Dashboard

#### 1. Go to Dashboard Studio
- Navigate to **Search & Reporting > Dashboards**
- Click **Create New Dashboard**
- Choose **Dashboard Studio**
- Set title, ID, and permissions
- Click **Create**


####  2. Add a Data Source (SPL)
Example SPL for live alert detail:

index=aws_security sourcetype=aws:guardduty
| spath
| search severity="$severity_filter$"
| table _time, title, severity, type, region, resource.resourceType
| rename title as "Alert Title", severity as Severity, type as "Alert Type", region as Region, resource.resourceType as "Resource Type"
