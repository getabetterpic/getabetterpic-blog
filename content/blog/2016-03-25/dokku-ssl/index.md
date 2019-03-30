---
layout: post
title: Deploying an SSL application to DigitalOcean with Dokku
tags:
  - dokku
  - digitalocean
  - digital ocean
  - ssl
  - ruby
  - rails
  - letsencrypt
author: Daniel Smith
published: true
date: '2016-03-25'
---

I love using Elastic Beanstalk on AWS, except for one thing. The cost. So I decided to try and get something similar to working with DigitalOcean using Dokku for deployment. This is how I did it.

First, create a [Dokku droplet in DO](https://www.digitalocean.com/community/tutorials/how-to-use-the-digitalocean-dokku-application). Make sure to check the `Use virtualhost naming for apps` box and put your root domain the `Hostname` field. No need to redirect your root domain to the IP as per the instructions, as long as the subdomain you want is pointed to the IP correctly.

Next, ssh into the droplet and create your Dokku application.

`dokku apps:create <name-of-app>`

Install the PostgreSQL plugin for Dokku:

`dokku plugin:install https://github.com/dokku/dokku-postgres.git
`

Then create the database image:

`dokku postgres:create <name-of-db-image>`

and link the two together:

`dokku postgres:link <name-of-db-image> <name-of-app>`

You now have an environment variable `DATABASE_URL` available to your app which will allow it to connect. If you have a Rails app, this would go in your database.yml file.

Now on your local machine, add the Dokku instance as a git remote:

`git remote add dokku dokku@<ip-of-droplet>:<name-of-app>`

`git push dokku master`

You should now have a running app in Dokku. You will need to ssh back into the Dokku server and run `dokku run <name-of-app> bundle exec rake db:migrate` to run migrations.

There are a couple of different ways to handle how your app gets run on the Dokku instance, by either using heroku buildpacks, or using a Dockerfile. I went the Dockerfile since I was already familiar with the technology.

I then used a [letsencrypt plugin](https://github.com/dokku/dokku-letsencrypt) for Dokku to get SSL up and running. On the Dokku machine:

`sudo dokku plugin:install https://github.com/dokku/dokku-letsencrypt.git`

At this point, you will need your DNS for your domain pointing at the IP of the droplet you are now using. Now run:

`dokku config:set --no-restart myapp DOKKU_LETSENCRYPT_EMAIL=your@email.tld`

One note here, the email after the `@` needs to include your subdomain.

Next:

`dokku letsencrypt myapp`

You should now have working SSL on your site. All for the price of a DO droplet.
