# test-gogs-drone-with-docker-compose

A very minimal test setup, with a single docker-compose.yml to spin up Gogs + Drone

## Setup / start

Start the servers with:

```
env DRONE_GOGS_URL=http://.... docker-compose up
```

Due to the fact that the docker-agent can't seem to be able to git clone from a local docker host (e.g. `http://gogs:3000/...`)
the best solution so far seems to be to use a non localhost gogs host.
This can be done with e.g. [https://ngrok.com/](https://ngrok.com/),
by starting a tunnel pointing to the local Gogs server (`./ngrok http 3000`) and then use the given `http://XXX.ngrok.io`
URL as the `DRONE_GOGS_URL` (e.g. `env http://XXX.ngrok.io docker-compose up`).

Then you can access the servers in your web browser:

- Gogs at: http://localhost:3000 (or http://XXX.ngrok.io)
- Drone at: http://localhost:8000

Open the Gogs server (http://localhost:3000 / http://XXX.ngrok.io) and select SQLite as the DB,
and set the host everywhere to the non localhost Gogs URL (e.g. http://XXX.ngrok.io).
Now create a user on Gogs - the first user created on Gogs is automatically an Admin userm, no email confirmation required.
Once created sign in with the user and create a repository.

You can now open Drone (http://localhost:8000) and log in with the Gogs user.


## Known issues

- After enabling a repo / adding a repo, the webhook auto registered by drone have to be changed,
  from "localhost:8000" to "drone-server:8000". Gogs can't send the webhook to localhost, as that would
  point inside that container instead of to the drone server container.
  You could of course create another public tunnel for the drone server, and specify that (http://XXX-2.ngrok.io)
  for the Drone host config (`docker-compose.yml`) instead of localhost, but simply changing the webhook
  domain on Gogs also works.


## Notes

- This example is for local testing only! `DRONE_SECRET` is baked into the `docker-compose.yml` config, you should not do this if the servers will be available publicly / outside of your computer!
