---
title: "Compatibility Testing Pulumi HCL"
url: "https://www.pulumi.com/blog/compatibility-testing-pulumi-hcl/"
date: "2026-08-14"
author: "Ian Wahbe"
feed_url: "https://www.pulumi.com/blog/rss.xml"
---
Pulumi HCL has at its core a simple promise: A program that works for tofu apply will also work for pulumi up . This must be true to allow Terraform modules to be shared between tofu config and Pulumi programs. This property makes testing Pulumi HCL simple.
