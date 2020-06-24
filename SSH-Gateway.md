SHS Gateway is build to help dev with app deploy and to access services in AWS through console (Only second account is available).

# How to use?
## Setting UP

You need to edit `~/.ssh/config` file and add this block:

```
Host oso2
  HostName 63.34.50.83
  User ubuntu
  ServerAliveInterval 300
  ForwardAgent yes
  IgnoreUnknown AddKeysToAgent,UseKeychain
  AddKeysToAgent yes
  UseKeychain yes
```

Host name can be changed depending on how you like it to be. 

## Deploy

When you ssh in, you will be logged in as ubuntu user. And you will be in ubuntu home directory. The OSO project is cloned into `~/oso` directory. You need to just cd into it `cd oso`. All usual commands will be available there. eg. `bin/projects/update`, `bin/start` or `bin/projects/deploy`.

> all details: 'how to deploy', you can find [here](https://github.com/resolving/oso/wiki/Deploy)