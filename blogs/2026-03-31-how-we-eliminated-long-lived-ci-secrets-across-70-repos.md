---
title: "How We Eliminated Long-Lived CI Secrets Across 70+ Repos"
url: "https://www.pulumi.com/blog/eliminating-ci-secrets-with-pulumi-esc/"
date: "Tue, 31 Mar 2026 00:00:00 +0000"
author: "Boris Schlosser"
feed_url: "https://www.pulumi.com/blog/rss.xml"
---
Supply chain attacks on CI/CD pipelines are accelerating. A growing pattern involves attackers compromising popular GitHub Actions through tag poisoning — rewriting trusted version tags to point to malicious code that harvests environment variables, cloud credentials, and API tokens from runner environments. The stolen credentials are then exfiltrated to attacker-controlled infrastructure, often before anyone notices.
