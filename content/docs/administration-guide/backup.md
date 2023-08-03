---
title: "Backup"
description: ""
summary: ""
date: 2023-05-15T12:16:32+02:00
lastmod: 2023-05-15T12:16:32+02:00
draft: false
images: []
menu:
  docs:
    parent: ""
    identifier: "backup-6773eb7de506682cac098bb1a83e1cdf"
weight: 810
toc: true
---

A backup of your database is all you need to be able to recreate your PostOwl in the event of a problem. All your content (indluding images) is in a single SQLite file.

## More backup options coming soon!

We've just launched PostOwl, so for now, backups are manual process. We'll be adding docs on other ways to backup (including using [litestram](https://litestream.io/)) very soon!

## Fly.io Database Backups

You can pull a backup locally and run it to check if it is valid. That's also quite useful for developing/testing against the latest production data.

1. Make a snapshot remotely
   - `fly ssh console`
   - `sqlite3 /data/db.sqlite3 ".backup /data/db.sqlite3.bacup"`
   - `sqlite3 /data/db.sqlite3.backup "PRAGMA integrity_check;"` (optional integrity check)
   - Exit the remote console (CTRL+D)
1. Download the database
   - `fly sftp get /data/db.sqlite3.backup data/db.sqlite3.backup`
   - you now have a local copy which you can restore later or rename to `db.sqlite3` and use locally for testing.

To restore a backup in production, you need to be a bit careful and follow these steps (your site could be down for a few minutes during the restore).

1. Make sure nobody writes to the app
1. Make a backup as shown above
1. Delete the existing production database files:
   - `fly ssh console`
   - `rm -rf /data/db.*`
   - Exit the remote console (CTRL+D)
1. Copy your local backup file to production using SFTP
   - `fly sftp shell`
   - `cd /data`
   - `put data/db.sqlite3.backup db.sqlite3`
   - Exit SFTP client (CTRL+D)
1. Restart the app (so that the new DB gets picked up)
   - `fly apps restart`

