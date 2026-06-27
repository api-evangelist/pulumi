---
title: "Introducing ESC Secret Rotation Webhooks"
url: "https://www.pulumi.com/blog/introducing-esc-secret-rotation-webhooks/"
date: "2026-06-26"
author: "Sean Yeh"
feed_url: "https://www.pulumi.com/blog/rss.xml"
---
Pulumi ESC centralizes your secrets and configuration, and it can automatically rotate secrets on a schedule so credentials never go stale. But a rotation is only useful if the systems that depend on it know it happened. ESC secret rotation webhooks close that gap by notifying you the moment a secret rotates.
