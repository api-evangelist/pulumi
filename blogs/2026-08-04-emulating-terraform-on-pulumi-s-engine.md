---
title: "Emulating Terraform on Pulumi's Engine"
url: "https://www.pulumi.com/blog/terraforms-data-model-on-pulumis-engine/"
date: "2026-08-04"
author: "Ian Wahbe"
feed_url: "https://www.pulumi.com/blog/rss.xml"
---
The core promise of Pulumi’s HCL support is that you can bring your existing Terraform configuration and modules, and pulumi will run them. If it works in OpenTofu and doesn’t work in Pulumi, we would like to fix that. Given that goal, our HCL interpreter needs to take HCL as input and emit instructions to the Pulumi engine that semantically match how tofu would interpret the same input.
