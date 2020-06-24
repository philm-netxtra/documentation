## About project.

Project’s idea is automated and systematized sorting and processing customers complaints in easy and convenient way. Developed Notifications system, with wide set of settings, conditions, selecting addressee, delivery channels etc.

## Architecture.

OSO project created with micro-service architecture. So at that moment we have:

* oso-spa (UI of the project, build on React. lerna-packages repo)
* oso-api (public facing API endpoints, comunicates with services to get/save service specific data, holds most of the business logic)
* decider-attachment-service (manages attachments)
* decider-evidence-service (manages evidence)
* decider-message-service (manages messaging)
* decider-note-service (manages notes)
* decider-task-service (manages tasks)
* decider-notification-service (manages notifications for app)
* oso-service-db (access to api db from services)
* oso-service-workers (delayed jobs for app, sends emails, OPSWAT check)
* oso-service-workflow (manages complaint workflow through aasm gem)
* oso-testing (integration tests)
* bunny_publisher (Rabbit MQ service, helps to make communication with rabbitMQ simple)
* endpoint-flux (organises enpoints into more simple way)

## Brief about users and users roles.

**Consumer** - client which create complaints. Complaints heading to dashboard.

**OS-users**:

- os-investigative-officer
- os-senior-investigative-officer
- os-admin
- os-senior-admin

**admin** - system admin

**company-user** - user for company

## Operation system setup (Ubuntu 18.04).

1. Docker, docker-compose.
https://docs.docker.com/install/linux/docker-ce/ubuntu/#install-docker-ce
https://docs.docker.com/compose/install/
Don’t forget to do: `$ sudo usermod -aG docker $USER`
2. Ruby – latest (2.5.3 now)
3. NodeJS and NPM - (10.14.2 and 6.4.1 respectively)
4. Git
5. AWS CLI

## Setup AWS
`~/.aws/config` should like that:    
```                             
[profile oso]  
  region = eu-west-1  
  output = json  
  aws_access_key_id = XXXXXXXXXXXXXXXXXXXXXx  
  aws_secret_access_key = XXXXXXXXXXXXXXXXXXXXX  
```

Check is a credentials are correct: `aws ecr get-login --no-include-email --profile oso`

Setup ecs-cli: https://docs.aws.amazon.com/AmazonECS/latest/developerguide/ECS_CLI_installation.html

**Initial actions and starting back-end.**

Project have a convenient and developed management system. It described in readme at github repo. So here I describe an order of actions for successful starting project.

1. `git clone git@github.com:resolving/oso.git` 
                   
   `git submodule update --init --recursive`

   `bin/projects/update --branch staging`
2. * go to github.com => Settings => Developer settings => Personal access tokens => Generate new token
   * Enter:
     - Token description: OSO developer token
     - Select: repo
   * copy/use token provided to oso/.env file to GITHUB_TOKEN=github_user_name:personal_access_token  
3. `$ bin/init` (create env files)
   Also need manually set keys for every service project:              
     AWS public key: AKIAJEZFGY4W5M6G6SMQ              
     AWS private key: kMWYJluKHsTkkmb6+zOYOr7kLMwxQ67lMf50BDWF              
4. `$ bin/setup` (create databases) 
5. `$ bin/start` (up project)
6. `$ bin/seed` (seed necessary data)
7. `$ bin/rake -- seed:test-data`


This will seed test data to the database, the active company will be 'Co-Operative Energy Limited'. Users and passwords (and other data) you can see in `projects/oso-api/db/seeds/test-data.yml` file.

## Important note.

* Folders in ~/docker-data contains mounted volumes for docker containers. These folders created under root account. And after this, services which uses dynamodb has failed (exited with code 1) after start.
In my case it was:                    
> docker-compose_decider-message-service  
> docker-compose_decider-evidence-service  
> docker-compose_decider-note-service  
> docker-compose_decider-attachment-service  

So its necessary to change owner of these files and folders to yourself.
Run 

```
sudo chown -R username:username ~/docker-data
```

* Also in development dont forget to set pool and MYSQL_POOL variables more than 20.

## Slack bot for bullet gem
For logging slow queries we use bullet gem. For non-dev environment it sends slack notifications. You can find more about configuring bullet and slack

* https://api.slack.com/apps/AT0TZK4D7 - slack bot
* https://github.com/flyerhzm/bullet - gem itself
* https://github.com/flyerhzm/uniform_notifier - gem for integrating notifications (slack and etc) and bullet

## Starting front-end.

```
$ cd projects/lerna-packages               
$ yarn && yarn bootstrap-all                   
$ yarn start-oso                
```

## Testing

```
$ bin/rspec --  --require rails_helper                
$ bin/rspec -- spec/endpoints/complaints/appeals/create_spec.rb --require rails_helper                 
```

Its a bit different for services, should add project:
`$ bin/rspec --projects oso-service-workers -- spec/workers/emails/any_to_comment_case_spec.rb --require spec_helper `                  

### For UI -
`$ yarn test-watch-oso`
from lerna-packages folder

## Usable links:

staging https://www.os-test.me/                

prod https://case.ombudsman-services.org/                 

sand https://sandbox.os-test.me/                 

EndpointFlux https://github.com/resolving/endpoint-flux/blob/master/README.md

Opswat https://github.com/resolving/oso/wiki/Installation-of-OPSWAT-MetaDefender-for-Windows

Bunny http://rubybunny.info/articles/getting_started.html
