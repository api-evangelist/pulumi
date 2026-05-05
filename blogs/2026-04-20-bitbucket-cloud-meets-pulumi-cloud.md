---
title: "Bitbucket Cloud Meets Pulumi Cloud"
url: "https://www.pulumi.com/blog/bitbucket-vcs-integration/"
date: "Mon, 20 Apr 2026 00:00:00 +0000"
author: "Luke Ward"
feed_url: "https://www.pulumi.com/blog/rss.xml"
---
<img src="https://www.pulumi.com/blog/bitbucket-vcs-integration/meta.png" />
<p>Pulumi Cloud now supports Bitbucket Cloud as a first-class VCS integration, joining <a href="https://www.pulumi.com/docs/version-control/github-app/">GitHub</a>, <a href="https://www.pulumi.com/docs/version-control/gitlab/">GitLab</a>, and <a href="https://www.pulumi.com/docs/version-control/azure-devops-integration/">Azure DevOps</a>. Connect your Bitbucket workspace to deploy infrastructure on every push, preview changes on pull requests, spin up ephemeral review stacks, and get AI-powered change summaries — all without an external CI/CD pipeline.</p>
<h2 id="deploy-infrastructure-from-bitbucket">Deploy infrastructure from Bitbucket</h2>
<p>Connect a Bitbucket repository to a stack and infrastructure deploys automatically when you push to your configured branch. Configure path filters so only relevant file changes trigger deployments, and manage environment variables and secrets directly in Pulumi Cloud. No external CI/CD pipeline required.</p>
<p>Every pull request gets an infrastructure preview showing exactly what will change before merging. <a href="https://www.pulumi.com/product/neo/">Neo</a> posts AI-generated summaries explaining what the changes mean in plain language, so reviewers can understand the impact without reading resource diffs.</p>
<h2 id="two-ways-to-connect">Two ways to connect</h2>
<p>The integration supports two authentication methods depending on your Bitbucket plan:</p>
<ul>
<li><strong>Personal OAuth</strong> works with every workspace, including free plans. Authorize through the standard OAuth flow and you&rsquo;re connected.</li>
<li><strong>Workspace tokens</strong> are available for Premium workspaces. Generate a token with the required scopes (<code>repository:admin</code>, <code>repository:write</code>, <code>pullrequest:write</code>, <code>webhook</code>) and paste it into Pulumi Cloud for a service-account-style connection that isn&rsquo;t tied to an individual user.</li>
</ul>
<p>Both methods register webhooks automatically — no manual configuration required.</p>
<h2 id="scaffold-new-projects-from-your-repositories">Scaffold new projects from your repositories</h2>
<p>The <a href="https://www.pulumi.com/docs/idp/concepts/new-project-wizard/">new project wizard</a> discovers your Bitbucket workspace, repositories, and branches so you can scaffold and deploy a new stack without leaving Pulumi Cloud. Create a new repository directly from the wizard or select an existing one and configure VCS-backed deployments in a few clicks.</p>
<h2 id="getting-started">Getting started</h2>
<ol>
<li>An org admin configures the integration under <strong>Management</strong> &gt; <strong>Version control</strong>.</li>
<li>Authorize with Bitbucket using personal OAuth or a workspace token.</li>
<li>Deploy infrastructure with first-class workflows.</li>
</ol>
<p>For full setup details, see the <a href="https://www.pulumi.com/docs/version-control/bitbucket/">Bitbucket integration docs</a>.</p>
<a class="btn btn-secondary whitespace-nowrap" href="https://app.pulumi.com/signin" rel="noopener noreferrer" target="_blank">
Connect your Bitbucket workspace
<i class="text-sm ml-2 fas fa-external-link-alt"></i>
</a>
