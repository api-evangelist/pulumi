---
title: "Pulumi Cloud REST API Docs, Now Generated from OpenAPI"
url: "https://www.pulumi.com/blog/rest-api-docs-from-openapi/"
date: "Mon, 20 Apr 2026 00:00:00 +0000"
author: "Devon Grove"
feed_url: "https://www.pulumi.com/blog/rss.xml"
---
<img src="https://www.pulumi.com/blog/rest-api-docs-from-openapi/meta.png" />
<p>The <a href="https://www.pulumi.com/docs/reference/cloud-rest-api/">Pulumi Cloud REST API reference</a> is now generated directly from the live <a href="https://api.pulumi.com/api/openapi/pulumi-spec.json">OpenAPI spec</a> at build time. Every endpoint, parameter, request body, and response schema you see on the page comes from the same spec the API itself publishes. The docs now stay in sync with the API automatically!</p>
<h2 id="why-this-matters">Why this matters</h2>
<p>The previous REST API reference was a set of handwritten pages. That meant every new endpoint, renamed parameter, or revised response shape needed a matching docs PR, and in practice the pages drifted. Small inconsistencies added up: missing parameters, outdated request shapes, schemas that no longer matched what the API returned. We wanted a durable fix that keeps the docs in sync as the API grows.</p>
<p>Generating the reference from the OpenAPI spec closes that gap. When the API ships a change, the docs pick it up automatically the next time our docs are built.</p>
<h2 id="whats-new">What&rsquo;s new</h2>
<p>The reference at <code>/docs/reference/cloud-rest-api/</code> now includes:</p>
<ol>
<li>Find what you need faster
<ul>
<li>Endpoints are grouped by product area — Stacks, Deployments, Environments, Organizations, Registry, Insights, AI, Workflows, and more — so you can jump straight to the part of the API you&rsquo;re working with.</li>
</ul>
</li>
<li>Complete request and response details
<ul>
<li>Every endpoint documents its parameters, request body, and the exact shape of what it returns, so you know what to send and what to expect back without guessing.</li>
</ul>
</li>
<li>One-click navigation between related types
<ul>
<li>When a response references another object, the type name is a link. Click through to drill into its full definition if desired instead of scrolling a lengthy API reference page.</li>
</ul>
</li>
</ol>
<h2 id="what-this-unlocks-for-agents">What this unlocks for agents</h2>
<p>Keeping the reference in sync with the spec isn&rsquo;t just a human convenience. It changes what&rsquo;s reliable for AI agents that read the docs and call the API on your behalf. An agent reading a handwritten reference might see a parameter that was renamed six months ago, or miss a field the API now returns, and the call fails silently or in ways that are hard to debug. When the reference is generated from the spec, the agent is working from what the API actually accepts today.</p>
<p>Say you&rsquo;re onboarding a new team and need to stand up their access in Pulumi Cloud. Point an agent at the REST API reference and ask it to create an <code>sre-oncall</code> team, add four members, and grant admin on three stacks. The agent walks the teams, memberships, and stack-permissions endpoints, builds the right sequence of calls, and executes.</p>
<p>The same pattern holds for bulk audits and cleanup. Ask an agent to find every stack in your org with no recent updates and tag them <code>stale</code>, and it can paginate correctly because the response schema matches reality. While workflows like these were technically possible before, they&rsquo;re much more reliable now.</p>
<h2 id="same-url-existing-links-keep-working">Same URL, existing links keep working</h2>
<p>The generated docs live at the same URL as the previous reference: <code>/docs/reference/cloud-rest-api/</code>. Bookmarks, blog links, and inbound search traffic still land on the right page. Redirects are in place for any API reference docs page that has been tweaked, renamed, or moved.</p>
<h2 id="try-it-out">Try it out</h2>
<p>Start at the new <a href="https://www.pulumi.com/docs/reference/cloud-rest-api/">REST API reference</a> and browse by category. Each page links through to the request and response object schemas it uses.</p>
<p>If you spot anything that looks wrong, the most likely culprit is the OpenAPI spec itself — file an issue in <a href="https://github.com/pulumi/docs">pulumi/docs</a> and we&rsquo;ll trace it back to the source. For tag intros and structural improvements, PRs to <a href="https://github.com/pulumi/docs">pulumi/docs</a> are welcome. Questions and feedback are always welcome in the <a href="https://slack.pulumi.com">Pulumi Community Slack</a>.</p>
<a class="btn btn-secondary whitespace-nowrap" href="https://www.pulumi.com/docs/reference/cloud-rest-api/">
Explore the REST API
</a>
