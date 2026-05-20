---
title: "Introducing Read-Only Mode for Pulumi Neo"
url: "https://www.pulumi.com/blog/neo-read-only-mode/"
date: "Wed, 01 Apr 2026 00:00:00 +0000"
author: "Florian Stadler"
feed_url: "https://www.pulumi.com/blog/rss.xml"
---
A platform engineer with broad access might want Neo to analyze infrastructure and suggest changes, but include guarantees it won’t actually apply them. Read-only mode makes that possible: Neo does the heavy lifting and hands off a pull request for your existing deployment process to pick up. Control what Neo can change Neo runs with the permissions of the user who creates a task, but you often want a tighter boundary.
