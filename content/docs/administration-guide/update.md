---
title: "Update"
description: ""
summary: ""
date: 2023-05-15T12:16:32+02:00
lastmod: 2023-05-15T12:16:32+02:00
draft: false
images: []
menu:
  docs:
    parent: ""
    identifier: "update-6773eb7de506682cac098bb1a83e1cdf"
weight: 110
toc: true
---

To update or upgrade PostOwl you will need to get the latest release from GitHub and deploy the changes.

<div class="alert alert-warning" role="alert">
  <p>⚠️ ATTENTION: PostOwl is currently in technical preview. Before updating make sure you have a backup file of your database.</p>
  <p>We don't currently have automated databse migrations, if there are database schema changes we will provide a script to run to update your database. Automated migrations will be available in a future release.</p>
  <p>Please check the release notes in GitHub before updating. We'll also share update instructions on the <a href="/blog">blog</a> and <a href="/newsletter">newsleter</a>).</p>
</div>

## Updating a fly.io deployment

To update to a new version on fly.io:

1. Pull the latest changes from GitHub to your local copy of the repo (you can pull a release or the `main` branch if you want to get the very latest changes).
2. Run `fly deploy` **with the options you used when doing the first deployment** see the [deploy](/docs/administration-guide/deploy/#deployment-to-flyio) for details.
