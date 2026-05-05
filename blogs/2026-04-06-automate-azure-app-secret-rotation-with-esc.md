---
title: "Automate Azure App Secret Rotation with ESC"
url: "https://www.pulumi.com/blog/automate-azure-app-secret-rotation-with-esc/"
date: "Mon, 06 Apr 2026 00:00:00 +0000"
author: "Sean Yeh"
feed_url: "https://www.pulumi.com/blog/rss.xml"
---
<img src="https://www.pulumi.com/blog/automate-azure-app-secret-rotation-with-esc/meta.png" />
<p><a href="https://learn.microsoft.com/en-us/entra/fundamentals/whatis">Microsoft Entra ID</a> (formerly Azure Active Directory) is Azure&rsquo;s identity and access management service. Any time your application needs to authenticate with Entra ID, you create an <strong>app registration</strong> and give it a <a href="https://learn.microsoft.com/en-us/entra/identity-platform/how-to-add-credentials?tabs=client-secret">client secret</a> that proves its identity. But those secrets expire, and if you don&rsquo;t rotate them in time, your app loses access.</p>
<p>If you or your team manages Azure app registrations, you know that keeping track of client secrets is a constant hassle. Forgetting to rotate them before they expire can lead to broken authentication and unexpected outages. With <a href="https://www.pulumi.com/docs/esc">Pulumi ESC</a>&rsquo;s <code>azure-app-secret</code> rotator, you can automate client secret rotation for your Azure apps, so you never have to worry about expired credentials again.</p>
<h2 id="setup">Setup</h2>
<h3 id="prerequisites">Prerequisites</h3>
<ul>
<li>An Azure App Registration</li>
<li>An <a href="https://www.pulumi.com/docs/esc/integrations/dynamic-login-credentials/azure-login/">azure-login</a> environment
<ul>
<li>Note for OIDC users: Since Azure does not support wildcard subject matches, you will need to add a <a href="https://www.pulumi.com/docs/esc/guides/configuring-oidc/azure/#add-federated-credentials">federated credential</a> for the azure-login environment as well as each environment that imports it.</li>
<li>The Azure identity used for rotation must have the <code>Application.ReadWrite.All</code> Graph API permission, or the identity must be added as an Owner of the specific app registration whose secrets will be rotated.</li>
</ul>
</li>
</ul>
<h2 id="how-it-works">How it works</h2>
<p>Let&rsquo;s assume your azure-login environment looks like this:</p>
<div class="highlight"><pre class="chroma" tabindex="0"><code class="language-yaml"><span class="line"><span class="cl"><span class="c"># my-org/logins/production</span><span class="w">
</span></span></span><span class="line"><span class="cl"><span class="nt">values</span><span class="p">:</span><span class="w">
</span></span></span><span class="line"><span class="cl"><span class="w"> </span><span class="nt">azure</span><span class="p">:</span><span class="w">
</span></span></span><span class="line"><span class="cl"><span class="w"> </span><span class="nt">login</span><span class="p">:</span><span class="w">
</span></span></span><span class="line"><span class="cl"><span class="w"> </span><span class="nt">fn::open::azure-login</span><span class="p">:</span><span class="w">
</span></span></span><span class="line"><span class="cl"><span class="w"> </span><span class="nt">clientId</span><span class="p">:</span><span class="w"> </span><span class="l">&lt;your-client-id&gt;</span><span class="w">
</span></span></span><span class="line"><span class="cl"><span class="w"> </span><span class="nt">tenantId</span><span class="p">:</span><span class="w"> </span><span class="l">&lt;your-tenant-id&gt;</span><span class="w">
</span></span></span><span class="line"><span class="cl"><span class="w"> </span><span class="nt">subscriptionId</span><span class="p">:</span><span class="w"> </span><span class="l">&lt;your-subscription-id&gt;</span><span class="w">
</span></span></span><span class="line"><span class="cl"><span class="w"> </span><span class="nt">oidc</span><span class="p">:</span><span class="w"> </span><span class="kc">true</span><span class="w">
</span></span></span></code></pre></div><p>Create a new environment for your rotator. If you have the existing credentials, set them in the <a href="https://www.pulumi.com/docs/esc/environments/syntax/builtin-functions/fn-rotate/#parameters">state</a> object so the rotator will treat them as the <code>current</code> credentials.</p>
<div class="highlight"><pre class="chroma" tabindex="0"><code class="language-yaml"><span class="line"><span class="cl"><span class="c"># my-org/rotators/secret-rotator</span><span class="w">
</span></span></span><span class="line"><span class="cl"><span class="nt">values</span><span class="p">:</span><span class="w">
</span></span></span><span class="line"><span class="cl"><span class="w"> </span><span class="nt">appSecret</span><span class="p">:</span><span class="w">
</span></span></span><span class="line"><span class="cl"><span class="w"> </span><span class="nt">fn::rotate::azure-app-secret</span><span class="p">:</span><span class="w">
</span></span></span><span class="line"><span class="cl"><span class="w"> </span><span class="nt">inputs</span><span class="p">:</span><span class="w">
</span></span></span><span class="line"><span class="cl"><span class="w"> </span><span class="nt">login</span><span class="p">:</span><span class="w"> </span><span class="l">${environments.logins.production.azure.login}</span><span class="w">
</span></span></span><span class="line"><span class="cl"><span class="w"> </span><span class="nt">clientId</span><span class="p">:</span><span class="w"> </span><span class="l">&lt;target-app-client-id&gt;</span><span class="w">
</span></span></span><span class="line"><span class="cl"><span class="w"> </span><span class="nt">lifetimeInDays</span><span class="p">:</span><span class="w"> </span><span class="m">180</span><span class="w"> </span><span class="c"># How long each new secret is valid (max 730 days)</span><span class="w">
</span></span></span><span class="line"><span class="cl"><span class="w"> </span><span class="nt">state</span><span class="p">:</span><span class="w">
</span></span></span><span class="line"><span class="cl"><span class="w"> </span><span class="nt">current</span><span class="p">:</span><span class="w">
</span></span></span><span class="line"><span class="cl"><span class="w"> </span><span class="nt">secretId</span><span class="p">:</span><span class="w"> </span><span class="l">&lt;secret-id&gt;</span><span class="w">
</span></span></span><span class="line"><span class="cl"><span class="w"> </span><span class="nt">secretValue</span><span class="p">:</span><span class="w">
</span></span></span><span class="line"><span class="cl"><span class="w"> </span><span class="nt">fn::secret</span><span class="p">:</span><span class="w"> </span><span class="l">&lt;secret-value&gt;</span><span class="w">
</span></span></span></code></pre></div><p>The <code>lifetimeInDays</code> field controls how long each generated secret remains valid before it expires. Azure allows a maximum of 730 days (two years), but shorter lifetimes are recommended for better security. Make sure to set a rotation <a href="https://www.pulumi.com/docs/esc/environments/rotation/#schedule">schedule</a> that runs before the lifetime expires so your credentials are always fresh.</p>
<p>Azure app registrations can have at most two client secrets at any given time, so the rotator maintains a <code>current</code> and <code>previous</code> secret. When a rotation occurs, the existing <code>current</code> secret becomes the <code>previous</code> secret, and a new secret is created to take its place as the new <code>current</code>. This ensures a smooth rollover with no downtime, since the previous secret remains valid until the next rotation.</p>
<p>Once this is set up, you&rsquo;re ready to go! You never need to worry about your client secrets expiring, and you will always have the latest credentials in your ESC Environment.</p>
<h2 id="learn-more">Learn more</h2>
<p>The <code>fn::rotate::azure-app-secret</code> rotator is available now in all Pulumi ESC environments. For more information, check out the <a href="https://www.pulumi.com/docs/esc/integrations/rotated-secrets/azure-app-secret/">fn::rotate::azure-app-secret documentation</a>!</p>
