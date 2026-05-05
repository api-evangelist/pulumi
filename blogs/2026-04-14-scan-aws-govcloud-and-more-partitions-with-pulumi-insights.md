---
title: "Scan AWS GovCloud and more partitions with Pulumi Insights"
url: "https://www.pulumi.com/blog/scan-aws-govcloud-china-with-pulumi-insights/"
date: "Tue, 14 Apr 2026 09:00:00 +0000"
author: "Alejandro Cotroneo"
feed_url: "https://www.pulumi.com/blog/rss.xml"
---
<img src="https://www.pulumi.com/blog/scan-aws-govcloud-china-with-pulumi-insights/meta.png" />
<p>Pulumi Insights account scanning now supports every AWS partition. If your workloads run in GovCloud, China, the European Sovereign Cloud, or one of the ISO intelligence-community clouds, you can get the same resource discovery, cross-account search, and AI-assisted insights that commercial accounts already have.</p>
<h2 id="supported-partitions">Supported partitions</h2>
<ol>
<li>AWS Standard (Commercial)</li>
<li>AWS GovCloud (US)</li>
<li>AWS ISO (US)</li>
<li>AWS ISOB (US)</li>
<li>AWS ISOF (US)</li>
<li>AWS ISOE (Europe)</li>
<li>AWS European Sovereign Cloud</li>
<li>AWS China</li>
</ol>
<p>You can also exclude specific regions from discovery — useful when regions are disabled by SCPs or fall outside an audit&rsquo;s scope.</p>
<p><img alt="Choosing an AWS partition when creating an Insights account" src="aws-partition-picker.png" /></p>
<h2 id="set-it-up">Set it up</h2>
<p>In the Pulumi Cloud console:</p>
<ol>
<li>Go to <strong>Accounts → Create account</strong>.</li>
<li>Select <strong>AWS</strong> as the provider.</li>
<li>Under <strong>Add your configuration</strong>, pick the target partition.</li>
<li>Supply credentials via a Pulumi ESC environment. The OIDC trust policy uses the partition-appropriate ARN prefix (<code>arn:aws-us-gov:</code>, <code>arn:aws-cn:</code>, etc.).</li>
</ol>
<p>For IAM and ESC setup, see the <a href="https://www.pulumi.com/docs/insights/discovery/accounts/">Insights accounts docs</a>. Log in to <a href="https://app.pulumi.com/">Pulumi Cloud</a> to get started.</p>
