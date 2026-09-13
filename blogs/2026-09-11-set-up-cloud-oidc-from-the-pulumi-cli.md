---
title: "Set Up Cloud OIDC From the Pulumi CLI"
url: "https://www.pulumi.com/blog/esc-oidc-setup-cli/"
date: "2026-09-11"
author: "Sean Yeh"
feed_url: "https://www.pulumi.com/blog/rss.xml"
---
Pulumi ESC can act as an OpenID Connect (OIDC) provider for AWS, Azure, and Google Cloud, issuing short-lived, signed tokens that these clouds exchange for temporary credentials. This eliminates hard-coded credentials and improves your security posture. Last year, we introduced an onboarding flow in the Pulumi Cloud console that makes it super easy to configure OIDC for your cloud provider in a few guided steps.
