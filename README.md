# test-gogs-drone-with-docker-compose

A very minimal test setup, with a single docker-compose.yml to spin up Gogs + Drone

Start with:

```
docker-compose up
```

## Notes

- This example is for local testing only! `DRONE_SECRET` is baked into the `docker-compose.yml` config, you should not do this if the servers will be available publicly / outside of your computer!
