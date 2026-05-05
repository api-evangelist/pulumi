---
title: "Neo's Integration Catalog: Give Your Agent Access to the Tools It Needs"
url: "https://www.pulumi.com/blog/neo-integration-catalog/"
date: "Fri, 24 Apr 2026 00:00:00 +0000"
author: "Pulumi Neo Team"
feed_url: "https://www.pulumi.com/blog/rss.xml"
---
<img src="https://www.pulumi.com/blog/neo-integration-catalog/meta.png" />
<p>Neo already helps your team manage Pulumi infrastructure, but no infrastructure team works inside Pulumi alone. Pages come from PagerDuty, telemetry from Datadog or Honeycomb, follow-ups from Linear or Jira. Most of the job is shuttling context between those tools.</p>
<p>Today we&rsquo;re launching the <strong>Integration Catalog</strong> for <a href="https://www.pulumi.com/blog/pulumi-neo/">Pulumi Neo</a>: one place to connect Neo to the tools your team already uses, so your agent has the context it needs to help.</p>
<h2 id="six-integrations-in-the-launch-catalog">Six integrations in the launch catalog</h2>
<p>Neo ships with six integrations at launch, each exposed to the agent through the <a href="https://modelcontextprotocol.io/">Model Context Protocol (MCP)</a>:</p>
<ul>
<li><strong>Atlassian</strong> — Jira issues, Confluence pages, project context</li>
<li><strong>Datadog</strong> — metrics, logs, monitors</li>
<li><strong>Honeycomb</strong> — traces and observability queries</li>
<li><strong>Linear</strong> — issue tracking and project workflows</li>
<li><strong>PagerDuty</strong> — incidents, on-call schedules, escalations</li>
<li><strong>Supabase</strong> — database management and edge functions</li>
</ul>
<p>Each integration is a remote MCP server. Neo calls the integration through a structured tool protocol and only sees the tools the vendor chooses to expose.</p>
<h2 id="neo-in-action-one-task-many-systems">Neo in action: one task, many systems</h2>
<p>A latency spike showed up in Datadog yesterday afternoon, and you want to know whether your deploy caused it.</p>
<blockquote>
<p><strong>You:</strong> Neo, our <code>payments</code> stack saw elevated p95 starting around 3pm yesterday. Did our deploy cause it? Check Datadog and Honeycomb.</p>
</blockquote>
<p>Neo lines up the Pulumi update history for the <code>payments</code> stack against the latency and error-rate metrics in Datadog around the same window, then surfaces the top slow traces in Honeycomb to confirm the suspect change.</p>
<blockquote>
<p><strong>You:</strong> Open a Linear ticket on the <code>platform</code> team with the findings and link the offending update.</p>
</blockquote>
<p>Neo opens the Linear issue with the summary, the Pulumi update URL, and a pointer to the Datadog dashboard, all without you leaving the chat or copy-pasting context between tabs.</p>
<h2 id="how-the-integration-catalog-works">How the Integration Catalog works</h2>
<p><strong>Admins configure credentials once.</strong> In your org&rsquo;s Neo settings, open the Integration Catalog, pick an integration, and paste in an API token or service-account key.</p>
<p><strong>Your team gets the capability immediately.</strong> No per-user setup, no extra OAuth flow for each developer, no asking platform to share a token in 1Password.</p>
<p><strong>Credentials stay encrypted at rest.</strong> When a task runs, the service decrypts the configured credentials just long enough to hand them to the agent runtime as MCP server auth.</p>
<h2 id="whats-coming-next-cli-oauth-and-access-controls">What&rsquo;s coming next: CLI, OAuth, and access controls</h2>
<p>This is the first cut. Here&rsquo;s what we&rsquo;re working on:</p>
<ul>
<li><strong>CLI integrations</strong> — give Neo access to command-line tools like <code>kubectl</code>, <code>aws</code>, <code>gcloud</code>, and <code>az</code>.</li>
<li><strong>OAuth integrations</strong> — for providers whose hosted MCP servers only speak OAuth (Notion, Sentry, Vercel), and for orgs that want per-user credentials.</li>
<li><strong>Per-integration access controls</strong> — team-scoped policies so admins can say &ldquo;only the platform team can let Neo touch PagerDuty.&rdquo;</li>
</ul>
<h2 id="try-it-out">Try it out</h2>
<p>The Integration Catalog is available now for Neo-enabled organizations. Open your org&rsquo;s Neo settings, head to the Integrations tab, and connect the first tool you reach for when something breaks. The <a href="https://www.pulumi.com/docs/ai/integrations/">Neo integrations docs</a> walk through the setup for each one.</p>
<p>As always, we&rsquo;d love to hear what&rsquo;s missing. File a feature request in <a href="https://github.com/pulumi/pulumi-cloud-requests/issues/new/choose">pulumi-cloud-requests</a> with the integration you want next. We&rsquo;re prioritizing based on what teams actually use.</p>
<p>Happy building.</p>
