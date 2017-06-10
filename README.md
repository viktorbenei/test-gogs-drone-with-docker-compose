# test-gogs-drone-with-docker-compose

A very minimal test setup, with a single docker-compose.yml to spin up Gogs + Drone

## Setup / start

Start the servers with:

```
docker-compose up
```

Then you can access the servers in your web browser:

- Gogs at: http://localhost:3000
- Drone at: http://localhost:8000

Open the Gogs server (http://localhost:3000) and select SQLite as the DB; everything else can be left on the default value.
Now create a user on Gogs - the first user created on Gogs is automatically an Admin userm, no email confirmation required.
Once created sign in with the user and create a repository.

You can now open Drone (http://localhost:8000) and log in with the Gogs user.


## Status

Base setup works, build is started, but **git clone fails** as for some reason drone tries to git clone through localhost,
while it should use `gogs` host (as configured in `docker-compose.yml` - `DRONE_GOGS_URL=http://gogs:3000`).

Also, after enabling a repo / adding a repo, the webhook auto registered by drone have to be changed,
from "localhost:8000" to "drone-server:8000".


## Notes

- This example is for local testing only! `DRONE_SECRET` is baked into the `docker-compose.yml` config, you should not do this if the servers will be available publicly / outside of your computer!
