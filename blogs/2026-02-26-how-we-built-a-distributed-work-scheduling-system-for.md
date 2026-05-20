---
title: "How We Built a Distributed Work Scheduling System for Pulumi Cloud"
url: "https://www.pulumi.com/blog/how-we-built-a-distributed-work-scheduling-system-for-pulumi-cloud/"
date: "Thu, 26 Feb 2026 00:00:00 +0000"
author: "Davide Massarenti"
feed_url: "https://www.pulumi.com/blog/rss.xml"
---
Pulumi Cloud orchestrates a growing number of workflow types: Deployments , Insights discovery scans, and policy evaluations . Some of that work runs on Pulumi’s infrastructure, and some of it runs on yours via customer-managed workflow runners . We needed a scheduling system that could handle all of these workflow types reliably across both environments.
