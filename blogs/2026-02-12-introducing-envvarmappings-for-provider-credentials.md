---
title: "Introducing envVarMappings for Provider Credentials"
url: "https://www.pulumi.com/blog/new-provider-resource-option-environment-variable-remapping/"
date: "Thu, 12 Feb 2026 00:00:00 +0000"
author: "Guinevere Saenger"
feed_url: "https://www.pulumi.com/blog/rss.xml"
---
Running multiple providers with different credentials in the same Pulumi program has always been tricky. Providers expect fixed environment variable names like AWS_ACCESS_KEY_ID or ARM_CLIENT_SECRET , so if you need two AWS providers targeting different accounts, you couldn’t configure them both via environment variables. Pulumi v3.220.0 introduces envVarMappings , a new resource option that solves this problem by letting you remap provider environment variables to custom keys.
