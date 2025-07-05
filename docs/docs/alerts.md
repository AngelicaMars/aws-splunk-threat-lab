## Testing Alerts in SPLUNK
- Example SPL & EC2 Activity Alerts/failed logins

## Suspiciouslogin_alert.spl
index=cloudtrail sourcetype=aws:cloudtrail eventName=GetCallerIdentity
| stats count by userIdentity.arn, sourceIPAddress, errorMessage, awsRegion

- Intentionally triggered multiple failed login attempts using the IAM user Splunk-reader in an incognito window.

- Both responseElements and requestParameters returned null.

- To improve clarity, I refined the view to display key details like time, username, IP address, and event parameters.
  
![image](https://github.com/user-attachments/assets/acbece4b-1ca6-4c69-ad67-938ef1f5e9a6)

 index=cloudtrail sourcetype=aws:cloudtrail eventName=GetCallerIdentity
| table _time, userIdentity.userName, userIdentity.arn, sourceIPAddress, eventType, requestParameters, responseElements
![image](https://github.com/user-attachments/assets/35ee072c-ce9a-40b7-953f-8b9d4baa39c4)
  
 - Made sure CloudTrail was receiving logs and working as expected.

![IAMfailedlogincloudtrail](https://github.com/user-attachments/assets/9f787dac-9c32-4b05-9574-60d73b4e653a)

## EC2Activity_Alert.spl

index=cloudtrail sourcetype=aws:cloudtrail eventSource=ec2.amazonaws.com
| stats count by eventName, userIdentity.arn

![EC2EditAlert](https://github.com/user-attachments/assets/7c91d1b0-cc7f-48dc-b621-c5dbf109eb86)

 - Spun up an EC2 instance to generate a detection event in Splunk. I ran the query above, saved it as an alert, and set the time range to 'Last 15 minutes' for a quick trigger—visible below.
   
![EC2activity detected](https://github.com/user-attachments/assets/95108510-bf67-42ad-b624-c643d518bdfe)


## Search Query in Splunk: EC2 Activity via CloudTrail Logs

 - This section covers how to search for EC2-related activity captured in AWS CloudTrail and visualized through Splunk.

![cloudtraiec2event](https://github.com/user-attachments/assets/48c3a53e-05c9-45b3-bcdd-97b1b7d46846)

## Basic Query – EC2 Event Activity
Search for events where the source is the EC2 service:

index=cloudtrail sourcetype=aws:cloudtrail eventSource=ec2.amazonaws.com

 - This returns a list of events including the time of occurrence, the action taken, and what specific activity was performed.

   ![image](https://github.com/user-attachments/assets/cf29779b-6556-4627-a68a-6cea0adc0c90)

## Refined Query – EC2 Event Summary Table
Use this query to create a cleaner summary of EC2 actions, including time, type of event, source, actor, and IP address:

index=cloudtrail sourcetype=aws:cloudtrail
| table _time eventName eventSource userIdentity.arn sourceIPAddress
| rename _time as "Time", eventName as "Event Type", eventSource as "Event Source", userIdentity.arn as "Actor", sourceIPAddress as "IP Address"

 - This table gives a clear overview of who did what, when, and from where regarding EC2 resources.

![image](https://github.com/user-attachments/assets/21431215-6e16-46e2-81ed-c622b7bdad7d)














  
