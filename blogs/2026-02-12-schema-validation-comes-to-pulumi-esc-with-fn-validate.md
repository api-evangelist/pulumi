---
title: "Schema Validation Comes to Pulumi ESC with fn::validate"
url: "https://www.pulumi.com/blog/esc-schema-validation-fn-validate/"
date: "Thu, 12 Feb 2026 11:00:00 -0300"
author: "Claire Gaestel"
feed_url: "https://www.pulumi.com/blog/rss.xml"
---
Pulumi ESC environments can now validate configuration values against JSON Schema with the new fn::validate built-in function. Invalid configurations are caught immediately when you save, preventing misconfigurations from reaching your deployments. Configuration errors are often discovered too late during deployment or, worse, in production.
