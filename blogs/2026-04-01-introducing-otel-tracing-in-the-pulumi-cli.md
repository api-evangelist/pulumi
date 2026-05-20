---
title: "Introducing OTel Tracing in the Pulumi CLI"
url: "https://www.pulumi.com/blog/introducing-otel-tracing-in-the-pulumi-cli/"
date: "Wed, 01 Apr 2026 00:00:00 +0000"
author: "Thomas Gummerer"
feed_url: "https://www.pulumi.com/blog/rss.xml"
---
Tracing is an important part of our CLI observability story. So far we’ve relied on (the now deprecated) OpenTracing for this. We have now added OTel tracing to the CLI, which is more future-proof, and should in most cases give you a better view over what the CLI is doing.
