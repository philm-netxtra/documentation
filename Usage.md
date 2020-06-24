# Usage
## `bin/be`

Runs bundle exec on box, default container is oso-api

```
bin/be -- rake seed:all
bin/be --projects oso-service-db -- rake -T
```

## `bin/clean`

Cleans all docker env.
- kills all running containers
- removes all containers
- removes all oso project images
- prunes docker system

## `bin/docker-compose`

Runs docker-compose with default config file.

```
bin/docker-compose up
bin/docker-compose build
bin/docker-compose --environment staging build # chooses docker compose file depending on env
```

## `bin/exec`

Runs docker-compose exec, default container is oso-api

```
bin/exec -- bundle exec rake seed:all
bin/exec --projects oso-service-db -- bundle exec rake -T
```

## `bin/init`

Runs `bin/init` in every project

```
bin/init # for all projects
bin/init --projects oso-api project2 # for one or more porjects
```

## `bin/kill`

Kills all running containers.

## `bin/migrate`

Runs migrate rake task, default container is oso-api

```
bin/migrate
bin/migrate --projects oso-service-db
```

## `bin/rails`

Runs rails, default container is oso-api

```
bin/rails -- console
bin/rails --projects decider-notification-service -- generate model Xxx
```

## `bin/rake`

Runs rake, default container is oso-api

```
bin/rake -- seed:all
bin/rake --projects oso-service-db -- search -T
```

## `bin/rspec`

Runs tests, default container is oso-api

```
bin/rspec
bin/rspec --projects oso-service-db -- spec/endpoint/individual_spec.rb:22
```

## `bin/run`

Runs docker-compose run, default container is oso-api

```
bin/run -- bundle exec rake seed:all
bin/run --projects oso-service-db -- bundle exec rake -T
```

## `bin/seed`

Runs `bundle exec rake seed:all`, default container is oso-api

```
bin/seed
bin/seed --projects oso-service-db
```

## `bin/setup`

Runs `bin/setup` in every project

```
bin/setup # for all projects
bin/setup --projects oso-api project2 # for one or more porjects
```

## `bin/start`

Starts project on docker

### `--projects` parameter

Limits containers to listed ones

```
bin/start --projects oso-api oso-service-db
```

## `bin/projects/update`

Checkouts and updates given branch on given projects

```
# updates staging on all projects
bin/projects/update
# checkouts master branch and updates it for oso-api and oso-service-db projects
bin/projects/update --branch master --projects oso-api oso-service-db
```