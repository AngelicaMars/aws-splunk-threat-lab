## Visualizations ##
This section covers the visualization of AWS GuardDuty findings in Splunk using custom SPL queries and 
Studio Dashboard panels. These visual insights help detect suspicious behavior, monitor threats over time, 
and identify targeted resources or IPs.




## Top 10 Most Frequent Threat Types

index=aws_security sourcetype=aws:guardduty
| stats count by detail.type
| sort -count
| head 10

 - Summary: Lists the most common types of GuardDuty findings.

- Trigger Use: Great for spotting recurring or trending threat types.

- Capability: Helps prioritize which types of threats your environment faces most.

- Visual: bar chart


## GuarDuty Alerts Over Time

index=aws_security sourcetype=aws:guardduty
| timechart span=1h count as "Detections"

- Summary: Displays the volume of GuardDuty alerts over time.

- Trigger Use: Detect spikes or quiet periods in threat activity.

- Capability: Supports anomaly detection and baselining.

- Visual: line chart

# IAM users triggering the most alerts

index=aws_security sourcetype=aws:guardduty
| spath path=resource.accessKeyDetails.userName output=user
| stats count as "Occurrences" by user
| sort -Occurrences
| head 10
| rename user as "IAM User"

- Summary: Lists IAM users or access keys triggering suspicious activity.

- Trigger Use: Flags compromised users or over-permissioned accounts.

- Capability: Supports auditing, threat hunting, and identity hardening.

- Visual: bar chart 


## Timeline of Alerts Over Time

index=aws_security sourcetype=aws:guardduty
| timechart span=1d count as "Alerts"

- Summary: Tracks volume of GuardDuty alerts daily.

- Trigger Use: Detects spikes, trends, attack campaigns

- Capability: Pairs beautifully with alerting and baseline monitoring.

- Visual: Column chart

