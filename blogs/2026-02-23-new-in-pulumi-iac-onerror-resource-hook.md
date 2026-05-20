---
title: "New in Pulumi IaC: `onError` Resource Hook"
url: "https://www.pulumi.com/blog/handling-deployment-errors/"
date: "Mon, 23 Feb 2026 00:00:00 +0000"
author: "Tom Harding"
feed_url: "https://www.pulumi.com/blog/rss.xml"
---
You can now control what happens when a resource fails during create, update, or delete—retry with backoff, fail fast, or handle errors in custom code. Last year, Pulumi IaC introduced the resource hooks feature, allowing you to run custom code at different points in the lifecycle of resources. Today we’re adding the onError hook so you can react when operations fail.
