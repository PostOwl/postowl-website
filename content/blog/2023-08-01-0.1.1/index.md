---
title: "PostOwl 0.1.1"
slug: "postowl-011"
description: ""
summary: ""
date: 2023-08-01T03:15:34+02:00
lastmod: 2023-08-01T03:15:34+02:00
draft: false
weight: 50
images: []
categories: []
tags: []
contributors: ["Tom Atkins"]
pinned: false
homepage: false
---

Thanks for everyone who tried the first release of PostOwl.

Today we're releasing some updates based on early feedback.

If you've already got a PostOwl deployment on fly.io you can update by pulling the latest changes and redeploying. Make sure to read the notes here as you'll need to edit some files and change your deployment command. (Future updates will be much simpler, so please bear with us for this release!)

## Simplified and more reliable deployment to fly.io

If you already have a PostOwl deployment, follow the steps below to update. (If it's your first time deploying, you can follow the [deployment docs](/docs/administration-guide/deploy/) and ignore this section.)

1. Get the latest release to your local repo: `git fetch --tags` then `git checkout tags/v0.1.1` (or you can `git pull` if you want to get the latest changes on `main`.)
1. Rename `.env.production.example` to `.env.production` and edit the values for your site (this means we won't need the 'BUILD-SECRETS' that were previously needed when deploying).
1. You'll need to make some changes to `fly.toml` (see the updated example in `fly.toml.example`). Specifically, change line 19 to read `cmd = ["start-fly.sh"]` and line 32 to read `auto_stop_machines = true`
1. Run `fly deploy`

That's it! Future deploys will just be getting the latest changes from the repo and running `fly deploy`!

## Scale to Zero on fly.io

'Scale to zero' means that when your site is inactive, the [machine will automatically shutdown](https://fly.io/docs/apps/autostart-stop/). This saves costs and electricity. For low traffic personal sites it's ideal.

In the past, the problem with shutting down is that it would take some time for your site to start up again - remember the Heroku free plan?! Fly.io use [firecracker vms](https://fly.io/docs/reference/architecture/#compute) which start almost instantly. If your site goes to sleep, you'll hardly notice it starting up. In our tests it takes about one second for the server to respond to an http request from a cold start! 🔥

The default configuration in PostOwl has `auto_stop_machines = true` in `fly.toml.example`. We recommend you keep this setting. We have code that gracefully closes the database connection and shuts down the application server before the machine stops.

Now you can have the benefits of a dynamic web application without the costs of keeping a VPS up all the time. And you're being kind to the environment! 🌳

## Minor changes

- Fixed search for non-signed in users
- Improved 'Send' and 'Save' buttons to be more context aware. We now show 'Publish' when making a letter public for the first time.
- Added 'New Letter' button in the header for signed-in users. Based on feedback this makes the main creative call to action obvious and gives a visual indicator that you're signed in.

Please [share any issues you find](https://github.com/PostOwl/postowl/issues) and [let us know your ideas for future development](https://github.com/PostOwl/postowl/discussions/categories/ideas)!

Thanks for trying PostOwl!
