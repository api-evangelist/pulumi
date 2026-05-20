---
title: "Lock Down Values in Pulumi ESC with fn::final"
url: "https://www.pulumi.com/blog/esc-fn-final/"
date: "Tue, 17 Mar 2026 11:00:00 -0700"
author: "Sean Yeh"
feed_url: "https://www.pulumi.com/blog/rss.xml"
---
Pulumi ESC (Environments, Secrets, and Configuration) allows you to compose environments by importing configuration and secrets from other environments, but this also means a child environment can silently override a value set by a parent. When that value is a security policy or a compliance setting, an accidental override can cause real problems. With the new fn::final built-in function, you can mark values as final, preventing child environments from overriding them.
