---
title: "Terraform and Kubernetes: A Practical Guide for 2026"
url: "https://www.pulumi.com/blog/terraform-kubernetes/"
date: "2026-08-07"
author: "Pulumi Content Team"
feed_url: "https://www.pulumi.com/blog/rss.xml"
---
Yes, Terraform can manage Kubernetes: the official hashicorp/kubernetes provider lets you declare Deployments, Services, and other objects as HCL resources, and community providers like kubectl fill in the gaps. It works well for many teams. The friction shows up around two well-documented limits — provider ordering and plan-time API access — and around testing, where a general-purpose language changes what’s possible.
