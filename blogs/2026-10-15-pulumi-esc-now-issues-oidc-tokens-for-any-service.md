---
title: "Pulumi ESC now issues OIDC tokens for any service"
url: "https://www.pulumi.com/blog/esc-oidc-tokens-for-any-service/"
date: "2026-10-15"
author: "Devon Grove"
feed_url: "https://www.pulumi.com/blog/rss.xml"
---
A Pulumi ESC environment can now mint a short-lived OpenID Connect (OIDC) token for any service that accepts one. Every pipeline and agent that touches your infrastructure eventually needs credentials for services beyond your cloud: a package registry such as Cloudsmith or JFrog Artifactory, an AI platform such as Anthropic, or a workload access platform such as Aembit. The default historically has been to store a long-lived API key that sits in an environment, which presents a much higher risk of exposure than short-lived credentials.
