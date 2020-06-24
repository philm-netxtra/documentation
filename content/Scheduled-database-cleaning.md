## Scheduled db cleaning consist of two parts.

### Backed part
Task for cleaning database is placed inside oso-api service:          
```rake complaints:remove_all[NUM]```          
where NUM is quantity of complaints for removing. Also it works without parameters, and remove all record in that case.


### AWS part
Settings for the scheduled tasks are located in AWS console, ECS services, oso-api-staging cluster tab.
Scheduled task called CleanDB.           
Current settings: 
* run every Friday at 19:00 ( cron(0 19 ? * FRI *) )
* run ``` bundle exec rake complaints:remove_all[30000]```


### Logs and output
Logs could be founded in Cloud Watch service in oso-api-staging group.


