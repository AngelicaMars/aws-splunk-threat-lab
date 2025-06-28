#Testing Alerts in SPLUNK
- Example SPL & failed logins

## login_alert.spl
index=aws sourcetype="aws:cloudtrail" eventName=ConsoleLogin
| stats count by userIdentity.arn, sourceIPAddress
| where count > 3

## First Search query in Splunk
| makeresults
| eval 
    eventName="ConsoleLogin", 
    userIdentity_arn="arn:aws:iam::111111111111:user/fakeuser", 
    responseElements_consolelogin="Failure", 
    sourceIPAddress="8.8.8.8", 
    awsRegion="us-east-1", 
    eventTime=strftime(now(), "%Y-%m-%dT%H:%M:%SZ")


## Second Search query in Splunk 
| makeresults
| eval 
    eventName="ConsoleLogin", 
    userIdentity_arn="arn:aws:iam::111111111111:user/fakeuser", 
    responseElements_consolelogin="Failure", 
    sourceIPAddress="8.8.8.8", 
    awsRegion="us-east-1", 
    eventTime=now()






  
