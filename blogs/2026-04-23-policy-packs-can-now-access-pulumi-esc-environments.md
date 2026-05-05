---
title: "Policy Packs Can Now Access Pulumi ESC Environments"
url: "https://www.pulumi.com/blog/policy-packs-can-now-access-pulumi-esc-environments/"
date: "Thu, 23 Apr 2026 00:00:00 +0000"
author: "Dan Biwer"
feed_url: "https://www.pulumi.com/blog/rss.xml"
---
<img src="https://www.pulumi.com/blog/policy-packs-can-now-access-pulumi-esc-environments/meta.png" />
<p>Policy authors who need external credentials or environment-specific configuration have had to hardcode values or manage them outside of Pulumi. Policy packs can now reference <a href="https://www.pulumi.com/product/secrets-management/">Pulumi ESC</a> environments, bringing centralized secrets and configuration management to your policies.</p>
<h2 id="the-problem">The problem</h2>
<p>Pulumi <a href="https://www.pulumi.com/docs/insights/policy/policy-packs/">policy packs</a> let you enforce rules across your infrastructure, but some policies need more than just the resource inputs they evaluate. A policy that validates resources against an external compliance API needs an API token. A cost-enforcement policy might need different spending thresholds for development and production environments. An access-control policy might need to reference an internal service registry.</p>
<p>Until now, these values had to be hardcoded in your policy group configuration or managed through a separate process entirely. This created several problems:</p>
<ul>
<li><strong>Security risk</strong>: Credentials stored in plain text in policy group config</li>
<li><strong>Operational burden</strong>: Updating a credential meant touching every policy group that used it</li>
<li><strong>No environment separation</strong>: The same values applied everywhere, with no way to vary configuration across environments</li>
</ul>
<h2 id="whats-new">What&rsquo;s new</h2>
<p>Policy packs can now reference ESC environments, just like <a href="https://www.pulumi.com/docs/esc/environments/syntax/reserved-properties/pulumi-config/">stacks already do</a>. When you attach an ESC environment to a policy pack in a policy group, the values from that environment are available to your policies at runtime — whether you&rsquo;re running preventative or audit policies.</p>
<p>This means your policy packs can use ESC for:</p>
<ul>
<li><strong>Secrets</strong>: API tokens, service credentials, and other sensitive values managed through ESC&rsquo;s secrets management, including dynamic credentials from providers like AWS, Azure, and GCP</li>
<li><strong>Configuration</strong>: Environment-specific thresholds, allowed regions, service allowlists, and other policy parameters that vary across environments</li>
</ul>
<p><img alt="Configuring ESC environments on a policy pack in the Pulumi Cloud console" src="policy-esc-screenshot.png" /></p>
<h2 id="how-it-works">How it works</h2>
<p>You configure ESC environment references on a policy pack within a policy group. At runtime, the values from those environments are resolved and made available to your policies through the policy pack&rsquo;s configuration.</p>
<p>Here&rsquo;s an example ESC environment that provides configuration to a compliance policy pack:</p>
<div class="highlight"><pre class="chroma" tabindex="0"><code class="language-yaml"><span class="line"><span class="cl"><span class="nt">values</span><span class="p">:</span><span class="w">
</span></span></span><span class="line"><span class="cl"><span class="w"> </span><span class="nt">compliance</span><span class="p">:</span><span class="w">
</span></span></span><span class="line"><span class="cl"><span class="w"> </span><span class="nt">apiToken</span><span class="p">:</span><span class="w">
</span></span></span><span class="line"><span class="cl"><span class="w"> </span><span class="nt">fn::secret</span><span class="p">:</span><span class="w"> </span><span class="l">xxxxxxxxxxxxxxxx</span><span class="w">
</span></span></span><span class="line"><span class="cl"><span class="w"> </span><span class="nt">costThreshold</span><span class="p">:</span><span class="w"> </span><span class="m">5000</span><span class="w">
</span></span></span><span class="line"><span class="cl"><span class="w">
</span></span></span><span class="line"><span class="cl"><span class="w"> </span><span class="nt">policyConfig</span><span class="p">:</span><span class="w">
</span></span></span><span class="line"><span class="cl"><span class="w"> </span><span class="nt">cost-compliance</span><span class="p">:</span><span class="w">
</span></span></span><span class="line"><span class="cl"><span class="w"> </span><span class="nt">maxMonthlyCost</span><span class="p">:</span><span class="w"> </span><span class="l">${compliance.costThreshold}</span><span class="w">
</span></span></span><span class="line"><span class="cl"><span class="w"> </span><span class="nt">apiEndpoint</span><span class="p">:</span><span class="w"> </span><span class="l">https://compliance.example.com</span><span class="w">
</span></span></span><span class="line"><span class="cl"><span class="w"> </span><span class="nt">apiToken</span><span class="p">:</span><span class="w"> </span><span class="l">${compliance.apiToken}</span><span class="w">
</span></span></span></code></pre></div><p>The <a href="https://www.pulumi.com/docs/esc/environments/syntax/reserved-properties/policy-config/"><code>policyConfig</code></a> property works just like <a href="https://www.pulumi.com/docs/esc/environments/syntax/reserved-properties/pulumi-config/"><code>pulumiConfig</code></a> does for stacks. Values nested under each policy name are made available as configuration to that policy at runtime. Secrets remain encrypted and are only decrypted when the environment is resolved.</p>
<p>You can also use the <code>environmentVariables</code> property to inject values as environment variables into the policy runtime, following the same pattern as <a href="https://www.pulumi.com/docs/esc/environments/syntax/reserved-properties/environment-variables/">stack environment variables</a>.</p>
<h2 id="example-compliance-api-validation">Example: compliance API validation</h2>
<p>Consider a policy that validates every new resource against an external compliance API before it can be provisioned. The API requires an authentication token and returns whether the resource configuration meets your organization&rsquo;s compliance standards.</p>
<p><strong>Before</strong>, the API token lived in the policy group configuration in plain text. Rotating the token meant updating every policy group. There was no audit trail for who accessed the credential, and no way to use different API endpoints for staging and production compliance checks.</p>
<p><strong>After</strong>, the API token lives in an ESC environment. You get:</p>
<ul>
<li><strong>Centralized rotation</strong>: Update the token in one place and every policy group that references the environment picks up the change</li>
<li><strong>Access controls</strong>: ESC&rsquo;s role-based access controls govern who can view or modify the credential</li>
<li><strong>Audit trail</strong>: Every access to the environment is logged</li>
<li><strong>Environment separation</strong>: Use different ESC environments for different policy groups, so staging policies validate against a staging compliance endpoint while production policies use the production endpoint</li>
</ul>
<h2 id="get-started">Get started</h2>
<p>To start using ESC environments with your policy packs:</p>
<ol>
<li><a href="https://www.pulumi.com/docs/esc/environments/working-with-environments/">Create an ESC environment</a> with your policy configuration and secrets</li>
<li>Attach the environment to a policy pack in your policy group through the Pulumi Cloud console</li>
<li>Update your policies to read from the configuration values provided by the environment</li>
</ol>
<p>To learn more:</p>
<ul>
<li><a href="https://www.pulumi.com/docs/esc/environments/syntax/reserved-properties/policy-config/"><code>policyConfig</code> reference</a></li>
<li><a href="https://www.pulumi.com/docs/esc/">Pulumi ESC documentation</a></li>
<li><a href="https://www.pulumi.com/docs/insights/policy/policy-packs/">Policy packs documentation</a></li>
<li><a href="https://www.pulumi.com/docs/esc/get-started/">Get started with Pulumi ESC</a></li>
</ul>
