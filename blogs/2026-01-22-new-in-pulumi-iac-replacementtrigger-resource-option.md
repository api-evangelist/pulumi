---
title: "New in Pulumi IaC: `replacementTrigger` Resource Option"
url: "https://www.pulumi.com/blog/triggering-resource-replacements/"
date: "Thu, 22 Jan 2026 00:00:00 +0000"
author: "Tom Harding"
feed_url: "https://www.pulumi.com/blog/rss.xml"
---
Pulumi IaC gives us a declarative interface to updates. When we perform an update, Pulumi calculates the difference between your currently deployed infrastructure and what is being proposed, then deploys only what is required to migrate from the old state to the new state. Normally, this is exactly what we want: we minimize the amount of work required to perform the update, and don’t recreate anything unnecessarily.
