# Docker configuration files for running a developement server for zammad (ticketing)

### Quick Steps:

- create a dir that you can later checkout the zammad source to (name does not matter)
- put `docker-compose.override.yml` & `docker-compose.yml` for this repo to that new dir
- `git clone` the zammand source into the same dir next to the `docker-compose.override.yml` / `docker-compose.yml` (git clone https://github.com/zammad/zammad)

-> you now should have a structure like that:

zammad-dev/         
│
├─ docker-compose.yml
├─ docker-compose.override.yml
│
└─ zammad/                  <-- zammad source (you checked out before)
    ├─ Dockerfile
    ├─ app/
    ├─ config/
    ├─ docker/
    ├─ ...

- copy `Dockerfile.dev` from this repo to the zammand source dir you checked out before
- copy `zammad.dev.conf` from this repo to `zammad\docker\nginx` (where again `zammad` is the source dir you checked out before)

- copy `config/database/database.yml` to `config/database.yml` and adjust it like so:
```yml
# ... shortened
default: &default
  pool: 50
  encoding: utf8
  adapter: postgresql
  username: zammad
  password: zammad
  host: postgres
  port: 5432
# ... shortened
```

- run `docker compose build` 
- run `docker compose run --rm rails bin/rails db:setup` & `docker compose run --rm rails bin/rails db:migrate` to setup the initial database
- start all containers using `docker compose up`

By now you should be able to access the dockerized dev env through `http://localhost:3000`

All code changes will now be observed and compiled/tanspiled and as you reload the dev URL you should see them.