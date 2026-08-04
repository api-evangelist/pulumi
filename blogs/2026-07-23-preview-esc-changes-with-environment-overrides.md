---
title: "Preview ESC Changes with Environment Overrides"
url: "https://www.pulumi.com/blog/preview-esc-environment-changes-with-draft-references/"
date: "2026-07-23"
author: "Sean Yeh"
feed_url: "https://www.pulumi.com/blog/rss.xml"
---
Pulumi ESC makes it easy to store configuration and secrets for your Pulumi programs, and with Approvals for ESC you can review and approve changes before they go live. The new --override-env flag lets you preview any environment change, including an unapproved draft, to see exactly how it would affect your stack before it becomes the latest version. Example scenario Your team stores production app configuration in ESC and has enabled Approvals to keep bad values out of critical infrastructure.
