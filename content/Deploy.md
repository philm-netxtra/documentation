## Check before deploy

Before any deploy (sandbox, academy, production) is always good to check the code which will be deployed to servers. Usually with `git diff`. Main things to take into account are:
* migrations.
* new rake tasks for data update.
* is there any version of gem changed.
* anything suspicious, what can break or make env slower. 

## Tags/Versions

All deploys are marked with tag which represents an app version. The version tag is build in following way:
* production: contains only two numbers eg. `v19.1`. First number increases when there are some additional features. If deploy contains only fixes or small changes then we change only second number.
* sandbox: always three numbers eg. `v19.1.1`, this one means that we did 1 sandbox deploy after `v19.1` deploy.
* staging: always four numbers eg. `v19.1.1.3`, this one means that we did 3 deploys to staging.

Before production deploy it will ask if it is mayor release or not.

## Prepare before deploy

It is always good to prepare deploy in previous day.
1) Check difference in code. Ask questions about migrations, rake tasks if looks like they can cause problems.
2) on SSH Gateway checkout OSO project. (`cd ~/oso; git pull;`)
3) on SSH Gateway build and upload docker images to ECR. (`cd ~/oso; bin/prepare-deploy-production;`)

## Deploy

In order to deploy you must complete previous topic: Prepare before deploy. When it is done, you can deploy. (`cd ~/oso; bin/deploy-production`)

> To deploy academy, you need to run `bin/prepare-deploy-academy` to prepare deploy if not done for production and `bin/deploy-academy` to deploy to academy.