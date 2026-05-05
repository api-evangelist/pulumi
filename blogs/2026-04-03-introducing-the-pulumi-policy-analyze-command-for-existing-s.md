---
title: "Introducing the pulumi policy analyze Command for Existing Stacks"
url: "https://www.pulumi.com/blog/pulumi-policy-analyze-existing-stacks/"
date: "Fri, 03 Apr 2026 00:00:00 +0000"
author: "Fraser Waters"
feed_url: "https://www.pulumi.com/blog/rss.xml"
---
<img src="https://www.pulumi.com/blog/pulumi-policy-analyze-existing-stacks/meta.png" />
<p>You can now run <a href="https://www.pulumi.com/docs/insights/policy/policy-packs/">policy packs</a> against your existing stack state without running your Pulumi program or making provider calls. The new <code>pulumi policy analyze</code> command evaluates your current infrastructure against local policy packs directly, turning policy validation into a fast, repeatable check.</p>
<h2 id="why-this-command-matters">Why this command matters</h2>
<p>Policy authoring and policy updates usually involve an iteration loop:</p>
<ol>
<li>Make a policy change.</li>
<li>Run a policy check.</li>
<li>Inspect violations or remediations.</li>
<li>Repeat until the policy behavior matches intent.</li>
</ol>
<p>Before this command, that loop often depended on <a href="https://www.pulumi.com/docs/iac/cli/commands/pulumi_preview/"><code>pulumi preview</code></a> or <a href="https://www.pulumi.com/docs/iac/cli/commands/pulumi_up/"><code>pulumi up</code></a>, which can be heavier than you need when your goal is validating policy logic against known state.</p>
<p>With <code>pulumi policy analyze</code>, you can evaluate your current stack state directly and quickly.</p>
<h2 id="basic-usage">Basic usage</h2>
<p>At minimum, provide a policy pack path and optionally a stack:</p>
<div class="highlight"><pre class="chroma" tabindex="0"><code class="language-bash"><span class="line"><span class="cl">pulumi policy analyze <span class="se">\
</span></span></span><span class="line"><span class="cl"> --policy-pack ./policy-pack <span class="se">\
</span></span></span><span class="line"><span class="cl"> --stack dev
</span></span></code></pre></div><p>You can also pass a config file for each policy pack:</p>
<div class="highlight"><pre class="chroma" tabindex="0"><code class="language-bash"><span class="line"><span class="cl">pulumi policy analyze <span class="se">\
</span></span></span><span class="line"><span class="cl"> --policy-pack ./policy-pack <span class="se">\
</span></span></span><span class="line"><span class="cl"> --policy-pack-config ./policy-config.dev.json <span class="se">\
</span></span></span><span class="line"><span class="cl"> --stack dev
</span></span></code></pre></div><p>If any mandatory policy violations are found, the command exits non-zero.</p>
<p>If remediation policies fire, those changes are reported in output, but stack state is not modified.</p>
<h2 id="testing-new-policy-packs-as-a-developer">Testing new policy packs as a developer</h2>
<p>For policy pack development, this command is useful as a tight local feedback loop:</p>
<ol>
<li>Pick a representative stack (<code>dev</code>, <code>staging</code>, or a fixture stack).</li>
<li>Run <code>pulumi policy analyze</code> against that stack after each policy change.</li>
<li>Use the output to verify mandatory, advisory, and remediation behavior.</li>
<li>Repeat before publishing the policy pack or attaching it to broader policy groups.</li>
</ol>
<p>Two output modes are especially useful:</p>
<ol>
<li><code>--diff</code> for a concise, human-readable view while iterating locally.</li>
<li><code>--json</code> for structured output that can be consumed in scripts and CI.</li>
</ol>
<h2 id="using-it-in-ai-and-agent-workflows">Using it in AI and agent workflows</h2>
<p>This command is also a good primitive for AI-assisted policy workflows.</p>
<p>Because <code>pulumi policy analyze</code> can emit JSON and a clear process exit code, agents can use it for deterministic policy evaluation steps:</p>
<ol>
<li>Propose or edit policy rules.</li>
<li>Run <code>pulumi policy analyze --json</code> against target stacks.</li>
<li>Parse violations and remediation signals.</li>
<li>Suggest policy fixes, config adjustments, or targeted infrastructure changes.</li>
<li>Re-run analysis until mandatory violations are resolved.</li>
</ol>
<p>For example, an agent tasked with fixing a policy violation can run <code>pulumi policy analyze --json</code> to get a structured list of violations, identify which resources are non-compliant, generate targeted infrastructure changes, then re-run analysis to confirm the violations are resolved, all without triggering a full preview on each iteration. The same loop works for policy authoring: an agent can propose a new policy rule, test it against several representative stacks, and surface unintended violations before the rule is published.</p>
<p>This works well for automation because the command doesn&rsquo;t execute your Pulumi program or make provider calls, so there are no side effects or runtime variance between runs. The JSON output and non-zero exit code on failure give agents a clear pass/fail contract to build on.</p>
<h2 id="try-it-out">Try it out</h2>
<p><code>pulumi policy analyze</code> is available in <a href="https://github.com/pulumi/pulumi/releases/tag/v3.229.0">Pulumi v3.229.0</a>.</p>
<p>If you are authoring or tuning policy packs, start by running this command against a known stack in your environment. It is a quick way to validate policy behavior before rollout.</p>
<p>For implementation details, see the merged PR: <a href="https://github.com/pulumi/pulumi/pull/22250">pulumi/pulumi#22250</a>.</p>
<a class="btn btn-secondary whitespace-nowrap" href="https://www.pulumi.com/docs/insights/policy/">
Get started with policy as code
</a>
