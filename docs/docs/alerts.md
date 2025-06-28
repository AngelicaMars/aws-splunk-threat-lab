## 6. Alerts & dashboards
- Example SPL for failed logins

## queries/login_alert.spl
index=aws sourcetype="aws:cloudtrail" eventName=ConsoleLogin
| stats count by userIdentity.arn, sourceIPAddress
| where count > 3

## Search query in Splunk 
| makeresults
| eval 
    eventName="ConsoleLogin", 
    userIdentity_arn="arn:aws:iam::111111111111:user/fakeuser", 
    responseElements_consolelogin="Failure", 
    sourceIPAddress="8.8.8.8", 
    awsRegion="us-east-1", 
    eventTime=now()






  
