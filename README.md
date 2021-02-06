# Installing GlitchTip on DigitalOcean App Platform

GlitchTip on DO App Platform consists of a web, worker, migration job, postgres database, and redis. Start by clicking the following button. Note this link acts as a referral and helps fund GlitchTip.

[![Deploy to DO](https://www.deploytodo.com/do-btn-blue.svg)](https://cloud.digitalocean.com/apps/new?repo=https://gitlab.com/glitchtip/glitchtip-app/tree/master&refcode=7e90b8fb37f8)

Leave environment variables blank and click next. Pick the basic or pro plan. One 512 MB RAM | 1 vCPU is fine to start with. Click Launch. Now copy [app-platform.yaml](app-platform.yaml) to your local computer. Edit the following

## Name

This can be anything. We default to "glitchtip"

## Environment Variables

At a minimum, set the SECRET_KEY to a random string of letters.

For more information on configuration settings see our install [docs](https://glitchtip.com/documentation/install#Configuration).

## Redis

GlitchTip requires Redis for sending notification, managing events, and more. Go to https://cloud.digitalocean.com/databases/ and create a new redis database. For almost all size instances, the 1 GB RAM | 1 vCPU instance is sufficient. Enter your redis database's name in the glitchtip-redis section. Let's assume it's named "glitchtip-redis". At the time of this writing, both "name" and "cluster_name" must be the same value. 

```
- name: glitchtip-redis
  engine: REDIS
  production: true
  cluster_name: glitchtip-redis
```

Ensure the environment variable "REDIS_URL" uses the same name. If you didn't name your redis instance "glitchtip-redis" then make sure to update it.

## Deploying

You'll need to install [doctl](https://www.digitalocean.com/docs/apis-clis/doctl/how-to/install/) and log in. 

Run `doctl apps list` to get your app's id.

Now apply your app-platform.yaml spec with `doctl apps update 11111111-1111-1111-1111-111111111 --spec app-platform.yaml` (enter your actual id)

After deployment, you should be able to visit the app URL and start using GlitchTip!

## Production considerations

If you intend to use GlitchTip in production, consider upgrading your Postgres database to a production instance. In the web interface, go to Manage Components, glitchtip-db, Upgrade to a managed database.

If you haven't already, you'll need to set up email via environment variables. 

## Upgrading GlitchTip

By default, the docker image tag is "latest". Click Deploy to upgrade to the latest GlitchTip docker image.
