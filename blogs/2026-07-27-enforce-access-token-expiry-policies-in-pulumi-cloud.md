---
title: "Enforce Access Token Expiry Policies in Pulumi Cloud"
url: "https://www.pulumi.com/blog/access-token-expiry-policy/"
date: "2026-07-27"
author: "Devon Grove"
feed_url: "https://www.pulumi.com/blog/rss.xml"
---
Pulumi Cloud organizations can now enforce a maximum expiry on the access tokens used against them. Organization admins can set a cap in days, and from that point on, personal, organization, and team tokens operating on resources in the org must carry an expiration within the cap for requests to succeed. Tokens that never expire, or that have too much lifetime remaining, get rejected with an error that tells the user exactly how to regain access.
