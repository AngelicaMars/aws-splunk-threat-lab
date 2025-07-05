## Visualize ##
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

- Visual: Use a bar chart or donut chart.


## GuarDuty Alerts Over Time

index=aws_security sourcetype=aws:guardduty
| timechart span=1h count as "Detections"

- Summary: Displays the volume of GuardDuty alerts over time.

- Trigger Use: Detect spikes or quiet periods in threat activity.

- Capability: Supports anomaly detection and baselining.

- Visual: Use a line chart, area chart, or single value with trend.

# Source IPs Triggering Alerts

index=aws_security sourcetype=aws:guardduty
| stats count by detail.service.action.remoteIpDetails.ipAddressV4
| sort -count
| rename detail.service.action.remoteIpDetails.ipAddressV4 as "Source IP"

- Summary: Lists IPs that triggered GuardDuty alerts, sorted by frequency.

- Trigger Use: Flag known malicious IPs or unusual access patterns.

- Capability: Can be paired with threat intel for deeper analysis.

- Visual: Use a table, bar chart, or geolocation map if IP-to-location enrichment is available.


## Top Targeted Resources

index=aws_security sourcetype=aws:guardduty
| stats count by detail.resource.instanceDetails.instanceId
| sort -count
| rename detail.resource.instanceDetails.instanceId as "Targeted Instance"

- Summary: Shows which EC2 instances or resources are being targeted most.

- Trigger Use: Focus on securing the most attacked resources.

- Capability: Helps with resource hardening and prioritization.

- Visual: Use a bar chart, single value, or table.

