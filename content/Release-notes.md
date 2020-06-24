# Release notes

### 27-05-2020, OSO-2954

Add OPSWAT_OPEN_READ_TIMEOUT = 120 variable to all task-definitions on AWS

 **Run on:**	
- [x] staging	
- [x] sandbox	
- [x] production
- [x] academy

### 15-05-2020, OSO-2939

Add BUNNY_KILL_ON_ERROR = true variable to all task-definitions on AWS

 **Run on:**	
- [x] staging	
- [x] sandbox	
- [x] production
- [x] academy

### 17-03-2020, OSO-2702

Following actions needed to be run after deploy:

```bash	
bin/deploy/run --environment {env} \	
               --projects oso-api \
               -- bundle exec rake complaint_histories:update_issued_next_action_at_timezone	
```
 **Run on:**	
- [x] staging	
- [x] sandbox	
- [x] production
- [x] academy

### 29-01-2020, OSO-2797

Following actions needed to be run after deploy:

```bash	
bin/deploy/run --environment {env} \	
               --projects oso-api \
               -- bundle exec rake statuses:update_2797	
```
 **Run on:**	
- [x] staging	
- [x] sandbox	
- [x] production
- [x] academy

### 15-08-2019, OSO-2466

Following actions needed to be run after deploy:

```bash	
bin/deploy/run --environment {env} \	
               --projects oso-api \
               -- bundle exec rake users:set_email_preference_for_consumers	
```

 **Run on:**	
- [x] staging	
- [x] sandbox	
- [x] production
- [x] academy


### 16-07-2019, OSO-2420 (TASK CONVERSION PART)

Following actions needed to be run after deploy:

```bash	
bin/deploy/run --environment {env} \	
               --projects decider-task-service \
               -- bundle exec rake tasks:update_parent_type	
```

NOTE: once it will be deployed to production, please save link to AWS logs for running task

 **Run on:**	
- [-] staging	
- [-] sandbox	
- [-] production
- [-] academy

### 25-06-2019, OSO-2334  (TASK CONVERSION PART)

Following actions needed to be run after deploy:

```bash	
bin/deploy/run --environment {env} \	
               --projects decider-task-service \
               -- bundle exec rake tasks:update_guide_guid	
```

NOTE: once it will be deployed to production, please save link to AWS logs for running task

 **Run on:**	
- [-] staging	
- [-] sandbox	
- [-] production
- [-] academy

### 25-06-2019, OSO-2346

Following actions needed to be run after deploy:

```bash	
bin/deploy/run --environment {env} \	
               --projects oso-api \
               -- bundle exec rake user_data:remove_invalid_phone_numbers	
```

NOTE: once it will be deployed to production, please save link to AWS logs for running task

 **Run on:**	
- [x] staging	
- [x] sandbox	
- [x] production
- [x] academy

### 18-05-2019, OSO-2295

Following actions needed to be run after deploy:

```bash	
bin/deploy/run --environment {env} \	
               --projects oso-api \
               -- bundle exec rake comments:update_accepted_for_proposal_comments	
```
AND

```bash	
bin/deploy/run --environment {env} \	
               --projects oso-api \
               -- bundle exec rake comments:update_accepted_for_dispute_comments	
```
 **Run on:**	
- [x] staging	
- [x] sandbox	
- [x] production
- [x] academy

### 10-04-2019, OSO-2004
Following actions needed to be run after deploy:

```bash	
bin/deploy/run --environment {env} \	
               --projects oso-api \
               -- bundle exec rake complaints:remove_duplicated_assignees	
```
 **Run on:**	
- [x] staging	
- [x] sandbox	
- [x] production
- [x] academy

### 21-03-2019, OSO-2179
Following actions needed to be run after deploy:

```bash	
bin/deploy/run --environment {env} \	
               --projects decider-notification-service \
               -- bundle exec rake move_data:receivers_flag_to_read_by_receivers	
```
 **Run on:**	
- [x] staging	
- [x] sandbox	
- [x] production
- [x] academy

### 19-03-2019, OSO-2155
Following actions needed to be run after deploy:

```bash	
bin/deploy/run --environment {env} \	
               --projects oso-api \	
               -- bundle exec rake users:set_proxy_for_self_created_consumers		
```
 **Run on:**	
- [x] staging	
- [x] sandbox	
- [x] production
- [x] academy

### 18-03-2019, OSO-2160
Following actions needed to be run after deploy:

```bash	
bin/deploy/run --environment {env} \	
               --projects oso-api \	
               -- bundle exec rake comments:update_type_and_parent		
```
 **Run on:**	
- [x] staging	
- [x] sandbox	
- [x] production
- [x] academy

### 26-01-2019, OSO-1995
Following actions needed to be run after deploy:

```bash	
bin/deploy/run --environment {env} \	
               --projects oso-api \	
               -- bundle exec rake complaints:update_status_details		
```
 **Run on:**	
- [x] staging	
- [x] sandbox	
- [x] production

### 24-01-2019, OSO-2009
Following actions needed to be run after deploy:

```bash	
bin/deploy/run --environment {env} \	
               --projects oso-api \	
               -- bundle exec rake complaints:update_usage_for_non_business_complaints		
```
 **Run on:**	
- [x] staging	
- [x] sandbox	
- [x] production

### 17-12-2018, OSO-1891
Following actions needed to be run after deploy:

```bash	
bin/deploy/run --environment {env} \	
               --projects oso-api \	
               -- bundle exec rake users:set_email_verified		
```

```bash	
bin/deploy/run --environment {env} \	
               --projects oso-api \	
               -- bundle exec rake tokens:remove_expired_tokens		
```
NOTICE! Order of tasks being executed matters!!

 **Run on:**	
- [x] staging	
- [x] sandbox	
- [x] production

 ### 04-12-2018, OSO-1866
  NOTICE! Long-running task. Updates every task in decider-task-service one by one.
  On staging, processing of 46908 tasks takes ~ 40 minutes

Following actions needed to be run after deploy:
	
```bash	
bin/deploy/run --environment {env} \	
               --projects decider-task-service \	
               -- bundle exec rake additional_info:set_complaint_reference		
```
 **Run on:**	
- [x] staging	
- [x] sandbox	
- [x] production


 ### 09-11-2018, OSO-1816

Following actions needed to be run after deploy:
	
```bash	
bin/deploy/run --environment {env} \	
               --projects oso-api \	
               -- bundle exec rake decision:update_attachments		
```	
 **Run on:**	
- [x] staging	
- [x] sandbox	
- [x] production

 ### 25-10-2018, OSO-1677
 
Following actions needed to be run after deploy:
	
```bash	
bin/deploy/run --environment {env} \	
               --projects decider-task-service \		
               -- bundle exec rake additional_info:set_complaint_status	
```
 **Run on:**	
- [x] staging	
- [x] sandbox	
- [x] production

 ### 11-10-2018, OSO-1672

Before deploy run one instance of workflow service. Currently is on 0.

 **Run on:**	
- [x] staging	
- [x] sandbox	
- [x] production	

 ### 09-10-2018, OSO-1701

Following actions needed to be run after deploy:
	
```bash	
bin/deploy/run --environment {env} \	
               --projects oso-api \	
               --started-by company-arrangement-populate \	
               -- bundle exec rake move_data:invoicing_name_to_arrangement	
```	
 **Run on:**	
- [x] staging	
- [x] sandbox	
- [x] production	
