---
title: "Trigger Deployments on Git Tags"
url: "https://www.pulumi.com/blog/trigger-deployments-on-git-tags/"
date: "2026-06-05"
author: "Michael Fallihee"
feed_url: "https://www.pulumi.com/blog/rss.xml"
---
Pulumi Deployments can now be configured to trigger automatically when git tags are pushed, allowing teams to run pulumi up on a stack whenever a version tag like v1.2.0 is created. Tag-based triggers support glob patterns for filtering and work across all five supported VCS integrations including GitHub, GitLab, Bitbucket, Azure DevOps, and custom VCS solutions. The tag name is available to programs via the PULUMI_CI_TAG_NAME environment variable for stamping version information onto deployed resources.
