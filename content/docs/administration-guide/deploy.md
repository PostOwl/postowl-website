---
title: "Deploy"
description: ""
summary: ""
date: 2023-05-15T12:16:32+02:00
lastmod: 2023-05-15T12:16:32+02:00
draft: false
images: []
menu:
  docs:
    parent: ""
    identifier: "deplpoy-6773eb7de506682cac098bb1a83e1cdf"
weight: 110
toc: true
---

<div class="alert alert-warning" role="alert">
  ⚠️ PostOwl currently needs some technical knowledge to deploy. We'll be launching a hosted version soon. <a href="/newsletter">Sign up for the newsletter</a> to hear when it's ready.
</div>

PostOwl is a [SvelteKit](https://kit.svelte.dev/) application inspired by [editable.website](https://editable.website) using [SQLite](https://www.sqlite.org/) for the database. It's currently optimised for SvelteKit's [adapter-node](https://github.com/sveltejs/kit/tree/master/packages/adapter-node) to enable [deployment to Fly.io](#deployment-to-flyio).

## More deployment options coming soon!

We'll be adding instructions for deploying to other hosts soon. If you have success deploying to other platforms, please [let us know](https://github.com/PostOwl/postowl/discussions/categories/show-and-tell) or submit a PR documenting how you did it.

## Sending email in production

PostOwl uses [nodemailer](https://nodemailer.com/about/) to send email notifications when you send letters to friends.

The application will run without a real SMTP server configured so **you can enter dummy data for the SMTP settings when deploying if you don't need to send emails**.

If you'd like to send emails but don't have an SMTP server to use, we recommend [mailgun](https://www.mailgun.com/). Their free tier will cover usage for a personal PostOwl site. You'll need to own a domain name to get set up with mailgun. Follow their documentation to activate your domain for email sending. Then generate an SMTP password from their admin interface (click 'Reset password' under Sending > Domain Settings > SMTP credentials to get a password for a new domain).

## Deployment to fly.io

**PostOwl runs for free on [fly.io](https://fly.io/)** which is one of the reasons we're recommending their platform for the first release. If you follow the steps here you should have a live site in about 15 minutes.

The [PostOwl repository](https://github.com/PostOwl/postowl) contains the files you need to deploy your PostOwl site to fly.io.

1. Create an account with [fly.io](https://fly.io/).
1. Add your credit card to Fly. PostOwl runs well on Fly's free tier but they [require an active, valid credit / bank card](https://fly.io/docs/about/credit-cards/) to prevent abuse. Unless you have a very busy site, hosting will be free.
1. [Install `flyctl`](https://fly.io/docs/hands-on/install-flyctl/) and sign in with `fly auth login`
1. Clone the PostOwl code to a directory on your computer: `git clone https://github.com/PostOwl/postowl.git`
1. Enter the directory you cloned the repo to: `cd postowl`
1. Rename `.env.production.example` to `.env.production`
1. Edit `.env.production`:
    1. Don't change the value of `DB_PATH`
    1. Edit all values with `your` in them (make sure to set a secure password)
    1. For the SMTP details, see the section above [Sending email in production](#sending-email-in-production). You don't need to use real values to try the app.
1. Run `fly launch` and respond to the prompts:
   1. Choose a name for your app (e.g. `yourapp`- app names need to be unique across all of fly.io) or hit enter to let Fly auto-generate a name
   1. Choose a Fly organization to deploy to if prompted
   1. Select the region to deploy to (Fly will automatically select one close to you)
1. Edit the generated `fly.toml` file. After the `[build]` section paste the lines below:  
  ```
[experimental]
  cmd = ["/app/scripts/start-fly.sh"]
  entrypoint = ["sh"]
```
10. Run `fly launch`

Fly will let you know when the app is deployed. Visit the URL shown in your terminal and sign in with the `ADMIN_PASSWORD` you set in `.env.production`.

Have fun creating letters! 🦉

### 'Scale to zero' on fly.io

'Scale to zero' means that when your site is inactive, the [machine will automatically shutdown](https://fly.io/docs/apps/autostart-stop/). This saves costs and electricity. For low traffic personal sites it's ideal.

In the past, the problem with shutting down is that it would take some time for your site to start up again - remember the Heroku free plan?! Fly.io use [firecracker vms](https://fly.io/docs/reference/architecture/#compute) which start almost instantly. If your site goes to sleep, you'll hardly notice it starting up. In our tests it takes about one second for the server to respond to an http request from a cold start! 🔥

The default configuration in PostOwl has `auto_stop_machines = true` in `fly.toml.example`. We recommend you keep this setting. We have code that gracefully closes the database connection and shuts down the application server before the machine stops.

Now you can have the benefits of a dynamic web application without the costs of keeping a VPS up all the time. And you're being kind to the environment! 🌳

### Connect a domain to your Fly.io app

You can serve PostOwl from a domain or subdomain you own.

- Run `fly ips list -a myapp` to get the IPv4 and IPv6 addresses.
- Head over to your DNS provider and add A and AAAA records for myapp.com with the IPv4 and IPv6 values.
- Run `fly certs create -a myapp myapp.com`
- Run `fly certs show -a myapp myapp.com` to watch your certificates being issued.

More details and considerations for subdomains in the [fly docs](https://fly.io/docs/app-guides/custom-domains-with-fly/)
