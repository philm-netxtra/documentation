Step by step instruction for production deploy.

# 1. Checkout and merge

```
bin/projects/update --branch sandbox
```

```
bin/projects/update --branch production
```

Checkout project to get the latest version of the sandbox and production. 

_Please read logs, sometimes there are some untracked changes in the projects and you can get error telling that. Commit or discard changes and then run the script again_

Then you need to **merge** the branch **sandbox** to **production** for each project, with default commit message like `merge sandbox`

```
cd projects/project-dir && git checkout production && git merge sandbox && git push
```

# 2. Check what changed

It is always good to go through code which will be deployed to prod. Main things you need to be careful about are:
* **Migrations**. Quite often it takes a long time to do, eg. creating index for big table. If there are potentially slow migrations raise discussion if it can be done in better way.
* **Rake tasks**. Check rake tasks, usually we do them after deploy, and they should not brake system. You need to make sure that the app not depending on data populated in rake task. If you see such dependencies, raise discussion about that with author or in dev chat. 
* **Gemfile changes**. Find out if gem update has no braking changes. Make sure if author checked that or check the gem change log.

It is better to ask author or in dev chat, if anything looks not right. **It is better to delay deploy than deploy a code you are not sure about.**

The changes can be found by finding last production tag (vNN.N) and doing `git diff ${version_tag}`.

Mark projects whit big changes, changes which contain more than small bug fixes. It will be needed in deployment process.

# 3. Preparing for deploy

You need to go to SSH Gateway. How to set up ssh to connect to it you can read [here](https://github.com/resolving/oso/wiki/SSH-Gateway). 

```
ssh oso2
ubuntu@ip-10-2-1-139:~$ cd oso
ubuntu@ip-10-2-1-139:~$ bin/projects/upload-images --environment production
```

You are ready to deploy after those commands are completed.

The `upload-images` script will build docker images and upload to amazon. The docker images will not be set as latest, so they will not go to prod. When you will run deploy script it will take much faster cause you will not need to build and upload images.

# 4. Deploy

Before starting deploy you need to set the maintenance screen by this command:
```
bin/projects/maintenance --environment production --on
```

The `aws-cli` has limits for checking deploy status. The limit is 10 services in same time. That's why we need to split our deploy into two parts. First one is to deploy services

```
bin/projects/deploy --environment production --projects decider-attachment-service decider-evidence-service decider-message-service decider-note-service decider-notification-service decider-task-service oso-service-db oso-service-workers oso-service-workflow
```

and second one is for SPA and API

```
bin/projects/deploy --environment production --projects oso-spa oso-api
```

Finally set the maintenance screen Off by this command:
```
bin/projects/maintenance --environment production --off
```

After deploy to prod is done, you need to deploy `academy` environment. That can be done with those commands:

```
bin/projects/deploy --environment academy --account oso2 --projects decider-attachment-service decider-evidence-service decider-message-service decider-note-service decider-notification-service decider-task-service oso-service-db oso-service-workers oso-service-workflow
bin/projects/deploy --environment academy --account oso2 --projects oso-spa oso-api
```

# 5. Run rake tasks

Go to this page [https://github.com/resolving/oso/wiki/Release-notes](https://github.com/resolving/oso/wiki/Release-notes) and check what needs to run after the deploy.
Mark them with [x] after task will have completed.