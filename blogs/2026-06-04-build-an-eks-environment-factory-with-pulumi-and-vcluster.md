---
title: "Build an EKS Environment Factory with Pulumi and vCluster"
url: "https://www.pulumi.com/blog/eks-vcluster-ephemeral-environments-with-pulumi/"
date: "2026-06-04"
author: "Pablo Seibelt"
feed_url: "https://www.pulumi.com/blog/rss.xml"
---
This article shows how to build scalable ephemeral Kubernetes environments using Pulumi, AWS EKS Auto Mode, and vCluster, consolidating multiple isolated virtual clusters onto a single host cluster. The environment factory pattern, as used by Deloitte to achieve 89% faster testing environment provisioning, lets platform teams give developers on-demand isolated Kubernetes environments without managing dozens of full clusters. The post covers host cluster setup with automatic node management, namespace-based tenant guardrails with resource quotas, and virtual cluster deployment via Helm.
