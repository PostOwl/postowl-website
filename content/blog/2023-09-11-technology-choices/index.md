---
title: "Technology Choices"
slug: "technology-choices"
description: ""
summary: "Why PostOwl is built with carefully chosen technology"
date: 2023-09-11T09:15:34+02:00
lastmod: 2023-09-11T09:15:34+02:00
draft: false
weight: 50
images: []
categories: []
tags: []
contributors: ["Tom Atkins"]
pinned: false
homepage: false
---

PostOwl has to be:

1. **Fast** (fast to edit content, fast to load pages, fast to search). See [Fast software, the best software](https://craigmod.com/essays/fast_software/).
1. **Easy to use** (easy to edit content, easy to read on any device)
1. **Efficient** (cheap to host, don't waste electricity when the site is idle)
1. **Long lasting**  (able to read your content in the future when technology has changed)

To meet these requirements PostOwl uses carefully chosen technology. We'll write in more detail about the various elements in the future, but here's a quick summary of how we achieve each goal:

1. **Fast**:
    1. [SvelteKit](https://kit.svelte.dev/) makes it easy to create a fast user interface. It feels magically fast when you first try it, even on modest hardware.
    1. [SQLite](https://www.sqlite.org) makes the database [as fast (or faster) than reading directly from the file system](https://www.sqlite.org/fasterthanfs.html)
1. **Easy to use**:
    1. [ProseMirror](https://prosemirror.net/) and [editable.website](https://editable.website/) techniques make 'in-place editing' a reality - no clunky admin interface. It's like using a Word Processor instead of a CMS.
    1. [Web standards](https://www.w3.org/standards) mean PostOwl can be usable and accessible on any device with a web browser (we still have work to do to make this better)
1. **Efficient**:
    1. You don't need a $5 per month VPS to host a PostOwl site. It'll happily [run on a free hosting plan](https://www.postowl.com/docs/administration-guide/deploy/#deployment-to-flyio).
    1. Enabling ['scale to zero'](https://www.postowl.com/docs/administration-guide/deploy/#scale-to-zero-on-flyio) with sub-second startup times means PostOwl doesn't waste electricity when your site is idle.
1. **Long lasting**:
    1. [SQLite is a Recommended Storage Format for datasets according to the US Library of Congress](https://www.sqlite.org/locrsf.html). Even if the code around PostOwl stops working, you'll be able to read your content long into the future using other tools (honourable mention to [Datasette](https://datasette.io/) here!)
    1. We use as few software dependencies as possible. [SvelteKit](https://kit.svelte.dev/) is the main dependency and they [work hard to reduce their own dependencies](https://svelte.dev/blog/svelte-4).
 
