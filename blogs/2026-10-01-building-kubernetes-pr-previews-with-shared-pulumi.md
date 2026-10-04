---
title: "Building Kubernetes PR Previews with Shared Pulumi Components"
url: "https://www.pulumi.com/blog/building-kubernetes-pr-previews-with-shared-pulumi-components/"
date: "2026-10-01"
author: "Sangharsh Agarwal"
feed_url: "https://www.pulumi.com/blog/rss.xml"
---
My team develops a microservices application on Kubernetes, with hundreds of PRs opened each day. To let engineers test and review those changes in isolation before they’re merged, we give every pull request its own ephemeral environment. We use Pulumi to define those short-lived PR environments from a component resource that’s shared with our long-lived Dev, Stage, Prod environments.
