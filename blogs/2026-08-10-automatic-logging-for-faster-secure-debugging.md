---
title: "Automatic Logging for Faster, Secure Debugging"
url: "https://www.pulumi.com/blog/automatic-logging/"
date: "2026-08-10"
author: "Thomas Gummerer"
feed_url: "https://www.pulumi.com/blog/rss.xml"
---
Pulumi v3.254.0 introduces automatic logging: every operation is logged in an encrypted log file that can optionally be shared with the Pulumi team for inspection. No more re-running commands just to get logs to the Pulumi team for debugging; instead you can share existing logs securely. You might have been in a situation where pulumi hit an error for an unexpected reason, or did something that was not quite right.
