---
title: "Introducing Bun as a Runtime for Pulumi"
url: "https://www.pulumi.com/blog/introducing-bun-as-a-runtime-for-pulumi/"
date: "Wed, 08 Apr 2026 00:00:00 +0000"
author: "Julien Poissonnier"
feed_url: "https://www.pulumi.com/blog/rss.xml"
---
<img src="https://www.pulumi.com/blog/introducing-bun-as-a-runtime-for-pulumi/meta.png" />
<p>Last year we added support for <a href="https://www.pulumi.com/blog/bun-package-manager/">Bun as a package manager</a> for Pulumi TypeScript projects. Today we&rsquo;re taking the next step: Bun is now a fully supported runtime for Pulumi programs. Set <code>runtime: bun</code> in your <code>Pulumi.yaml</code> and Bun will execute your entire Pulumi program, with no Node.js required. Since Bun&rsquo;s 1.0 release, this has been one of our <a href="https://github.com/pulumi/pulumi/issues/13904">most requested features</a>.</p>
<h2 id="why-bun">Why Bun?</h2>
<p><a href="https://bun.sh/">Bun</a> is a JavaScript runtime designed as an all-in-one toolkit: runtime, package manager, bundler, and test runner. For Pulumi users, the most relevant advantages are:</p>
<ul>
<li><strong>Native TypeScript support</strong>: Bun runs TypeScript directly without requiring <a href="https://typestrong.org/ts-node/">ts-node</a> or a separate compile step.</li>
<li><strong>Fast package management</strong>: Bun&rsquo;s built-in package manager can install dependencies significantly faster than npm.</li>
<li><strong>Node.js compatibility</strong>: Bun <a href="https://bun.sh/docs/runtime/nodejs-apis">aims for 100% Node.js compatibility</a>, so the npm packages you already use with Pulumi should work out of the box.</li>
</ul>
<p>With <code>runtime: bun</code>, Pulumi uses Bun for both running your program and managing your packages, giving you a streamlined single-tool experience.</p>
<h2 id="getting-started">Getting started</h2>
<p>To create a new Pulumi project with the Bun runtime, run:</p>
<div class="highlight"><pre class="chroma" tabindex="0"><code class="language-bash"><span class="line"><span class="cl">pulumi new bun
</span></span></code></pre></div><p>This creates a TypeScript project configured to use Bun. The generated <code>Pulumi.yaml</code> looks like this:</p>
<div class="highlight"><pre class="chroma" tabindex="0"><code class="language-yaml"><span class="line"><span class="cl"><span class="nt">name</span><span class="p">:</span><span class="w"> </span><span class="l">my-bun-project</span><span class="w">
</span></span></span><span class="line"><span class="cl"><span class="nt">runtime</span><span class="p">:</span><span class="w"> </span><span class="l">bun</span><span class="w">
</span></span></span></code></pre></div><p>From here, write your Pulumi program as usual. For example, to create a random password using the <code>@pulumi/random</code> package:</p>
<div class="highlight"><pre class="chroma" tabindex="0"><code class="language-bash"><span class="line"><span class="cl">bun add @pulumi/random
</span></span></code></pre></div><div class="highlight"><pre class="chroma" tabindex="0"><code class="language-typescript"><span class="line"><span class="cl"><span class="kr">import</span> <span class="o">*</span> <span class="kr">as</span> <span class="nx">random</span> <span class="kr">from</span> <span class="s2">"@pulumi/random"</span><span class="p">;</span>
</span></span><span class="line"><span class="cl">
</span></span><span class="line"><span class="cl"><span class="kr">const</span> <span class="nx">password</span> <span class="o">=</span> <span class="k">new</span> <span class="nx">random</span><span class="p">.</span><span class="nx">RandomPassword</span><span class="p">(</span><span class="s2">"password"</span><span class="p">,</span> <span class="p">{</span>
</span></span><span class="line"><span class="cl"> <span class="nx">length</span>: <span class="kt">20</span><span class="p">,</span>
</span></span><span class="line"><span class="cl"><span class="p">});</span>
</span></span><span class="line"><span class="cl">
</span></span><span class="line"><span class="cl"><span class="kr">export</span> <span class="kr">const</span> <span class="nx">pw</span> <span class="o">=</span> <span class="nx">password</span><span class="p">.</span><span class="nx">result</span><span class="p">;</span>
</span></span></code></pre></div><p>Then deploy with:</p>
<div class="highlight"><pre class="chroma" tabindex="0"><code class="language-bash"><span class="line"><span class="cl">pulumi up
</span></span></code></pre></div><p><strong>Prerequisites:</strong></p>
<ul>
<li><a href="https://bun.sh/docs/installation">Bun</a> 1.3 or later</li>
<li><a href="https://www.pulumi.com/docs/iac/download-install/">Pulumi</a> 3.227.0 or later</li>
</ul>
<h2 id="converting-existing-nodejs-projects">Converting existing Node.js projects</h2>
<p>If you have an existing Pulumi TypeScript project running on Node.js, you can convert it to use the Bun runtime in a few steps.</p>
<h3 id="1-update-pulumiyaml">1. Update <code>Pulumi.yaml</code></h3>
<p>Change the <code>runtime</code> field from <code>nodejs</code> to <code>bun</code>:</p>
<p>Before:</p>
<div class="highlight"><pre class="chroma" tabindex="0"><code class="language-yaml"><span class="line"><span class="cl"><span class="nt">runtime</span><span class="p">:</span><span class="w">
</span></span></span><span class="line"><span class="cl"><span class="w"> </span><span class="nt">name</span><span class="p">:</span><span class="w"> </span><span class="l">nodejs</span><span class="w">
</span></span></span><span class="line"><span class="cl"><span class="w"> </span><span class="nt">options</span><span class="p">:</span><span class="w">
</span></span></span><span class="line"><span class="cl"><span class="w"> </span><span class="nt">packagemanager</span><span class="p">:</span><span class="w"> </span><span class="l">npm</span><span class="w">
</span></span></span></code></pre></div><p>After:</p>
<div class="highlight"><pre class="chroma" tabindex="0"><code class="language-yaml"><span class="line"><span class="cl"><span class="nt">runtime</span><span class="p">:</span><span class="w"> </span><span class="l">bun</span><span class="w">
</span></span></span></code></pre></div><div class="note note-info">
<div class="icon-and-line">
<i class="fas fa-info-circle"></i>
<div class="line"></div>
</div>
<div class="content">When the runtime is set to <code>bun</code>, Bun is also used as the package manager — there&rsquo;s no need to configure a separate <code>packagemanager</code> option.</div>
</div>
<h3 id="2-update-tsconfigjson">2. Update <code>tsconfig.json</code></h3>
<p>Bun handles TypeScript differently from Node.js with <code>ts-node</code>. Update your <code>tsconfig.json</code> to use <a href="https://bun.sh/docs/typescript#suggested-compileroptions">Bun&rsquo;s recommended compiler options</a>:</p>
<div class="highlight"><pre class="chroma" tabindex="0"><code class="language-json"><span class="line"><span class="cl"><span class="p">{</span>
</span></span><span class="line"><span class="cl"> <span class="nt">"compilerOptions"</span><span class="p">:</span> <span class="p">{</span>
</span></span><span class="line"><span class="cl"> <span class="nt">"lib"</span><span class="p">:</span> <span class="p">[</span><span class="s2">"ESNext"</span><span class="p">],</span>
</span></span><span class="line"><span class="cl"> <span class="nt">"target"</span><span class="p">:</span> <span class="s2">"ESNext"</span><span class="p">,</span>
</span></span><span class="line"><span class="cl"> <span class="nt">"module"</span><span class="p">:</span> <span class="s2">"Preserve"</span><span class="p">,</span>
</span></span><span class="line"><span class="cl"> <span class="nt">"moduleDetection"</span><span class="p">:</span> <span class="s2">"force"</span><span class="p">,</span>
</span></span><span class="line"><span class="cl"> <span class="nt">"moduleResolution"</span><span class="p">:</span> <span class="s2">"bundler"</span><span class="p">,</span>
</span></span><span class="line"><span class="cl"> <span class="nt">"allowJs"</span><span class="p">:</span> <span class="kc">true</span><span class="p">,</span>
</span></span><span class="line"><span class="cl"> <span class="nt">"allowImportingTsExtensions"</span><span class="p">:</span> <span class="kc">true</span><span class="p">,</span>
</span></span><span class="line"><span class="cl"> <span class="nt">"verbatimModuleSyntax"</span><span class="p">:</span> <span class="kc">true</span><span class="p">,</span>
</span></span><span class="line"><span class="cl"> <span class="nt">"strict"</span><span class="p">:</span> <span class="kc">true</span><span class="p">,</span>
</span></span><span class="line"><span class="cl"> <span class="nt">"skipLibCheck"</span><span class="p">:</span> <span class="kc">true</span><span class="p">,</span>
</span></span><span class="line"><span class="cl"> <span class="nt">"noFallthroughCasesInSwitch"</span><span class="p">:</span> <span class="kc">true</span><span class="p">,</span>
</span></span><span class="line"><span class="cl"> <span class="nt">"noUncheckedIndexedAccess"</span><span class="p">:</span> <span class="kc">true</span><span class="p">,</span>
</span></span><span class="line"><span class="cl"> <span class="nt">"noImplicitOverride"</span><span class="p">:</span> <span class="kc">true</span>
</span></span><span class="line"><span class="cl"> <span class="p">}</span>
</span></span><span class="line"><span class="cl"><span class="p">}</span>
</span></span></code></pre></div><p>Key differences from a typical Node.js <code>tsconfig.json</code>:</p>
<ul>
<li><a href="https://www.typescriptlang.org/tsconfig/#module"><code>module: &quot;Preserve&quot;</code></a> and <a href="https://devblogs.microsoft.com/typescript/announcing-typescript-5-0/#moduleresolution-bundler"><code>moduleResolution: &quot;bundler&quot;</code></a>: Let Bun handle module resolution instead of compiling to CommonJS. The <code>bundler</code> resolution strategy allows extensionless imports while still respecting <code>package.json</code> exports, matching how Bun resolves modules in practice.</li>
<li><a href="https://www.typescriptlang.org/tsconfig/#verbatimModuleSyntax"><code>verbatimModuleSyntax: true</code></a>: Enforces consistent use of ESM <code>import</code>/<code>export</code> syntax. TypeScript will flag any remaining CommonJS patterns like <code>require()</code> at compile time.</li>
</ul>
<h3 id="3-switch-to-esm">3. Switch to ESM</h3>
<p>Bun makes it easy to go full ESM and it&rsquo;s the <a href="https://bun.sh/docs/runtime/module-resolution">recommended module format</a> for Bun projects. Add <code>&quot;type&quot;: &quot;module&quot;</code> to your <code>package.json</code>:</p>
<div class="highlight"><pre class="chroma" tabindex="0"><code class="language-json"><span class="line"><span class="cl"><span class="p">{</span>
</span></span><span class="line"><span class="cl"> <span class="nt">"type"</span><span class="p">:</span> <span class="s2">"module"</span>
</span></span><span class="line"><span class="cl"><span class="p">}</span>
</span></span></code></pre></div><p>With <a href="https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules">ECMAScript module (ESM)</a> syntax, one thing that gets easier is working with async code. In a CommonJS Pulumi program, if you need to await a data source or other async call before declaring resources, the program must be wrapped in an <a href="https://www.pulumi.com/docs/iac/languages-sdks/javascript/#enabling-async-support">async entrypoint function</a>. With ESM and Bun, <a href="https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/await#top_level_await">top-level await</a> just works, so you can skip the wrapper function entirely and <code>await</code> directly at the module level:</p>
<div class="highlight"><pre class="chroma" tabindex="0"><code class="language-typescript"><span class="line"><span class="cl"><span class="kr">import</span> <span class="o">*</span> <span class="kr">as</span> <span class="nx">aws</span> <span class="kr">from</span> <span class="s2">"@pulumi/aws"</span><span class="p">;</span>
</span></span><span class="line"><span class="cl">
</span></span><span class="line"><span class="cl"><span class="kr">const</span> <span class="nx">azs</span> <span class="o">=</span> <span class="k">await</span> <span class="nx">aws</span><span class="p">.</span><span class="nx">getAvailabilityZones</span><span class="p">({</span> <span class="nx">state</span><span class="o">:</span> <span class="s2">"available"</span> <span class="p">});</span>
</span></span><span class="line"><span class="cl">
</span></span><span class="line"><span class="cl"><span class="kr">const</span> <span class="nx">buckets</span> <span class="o">=</span> <span class="nx">azs</span><span class="p">.</span><span class="nx">names</span><span class="p">.</span><span class="nx">map</span><span class="p">(</span><span class="nx">az</span> <span class="o">=&gt;</span> <span class="k">new</span> <span class="nx">aws</span><span class="p">.</span><span class="nx">s3</span><span class="p">.</span><span class="nx">BucketV2</span><span class="p">(</span><span class="sb">`my-bucket-</span><span class="si">${</span><span class="nx">az</span><span class="si">}</span><span class="sb">`</span><span class="p">));</span>
</span></span><span class="line"><span class="cl">
</span></span><span class="line"><span class="cl"><span class="kr">export</span> <span class="kr">const</span> <span class="nx">bucketNames</span> <span class="o">=</span> <span class="nx">buckets</span><span class="p">.</span><span class="nx">map</span><span class="p">(</span><span class="nx">b</span> <span class="o">=&gt;</span> <span class="nx">b</span><span class="p">.</span><span class="nx">id</span><span class="p">);</span>
</span></span></code></pre></div><p>If your existing program does use an async entrypoint with <code>export =</code>, just replace it with the ESM-standard <code>export default</code>:</p>
<div class="highlight"><pre class="chroma" tabindex="0"><code class="language-typescript"><span class="line"><span class="cl"><span class="c1">// CommonJS (Node.js default)
</span></span></span><span class="line"><span class="cl"><span class="kr">export</span> <span class="o">=</span> <span class="kr">async</span> <span class="p">()</span> <span class="o">=&gt;</span> <span class="p">{</span>
</span></span><span class="line"><span class="cl"> <span class="kr">const</span> <span class="nx">bucket</span> <span class="o">=</span> <span class="k">new</span> <span class="nx">aws</span><span class="p">.</span><span class="nx">s3</span><span class="p">.</span><span class="nx">BucketV2</span><span class="p">(</span><span class="s2">"my-bucket"</span><span class="p">);</span>
</span></span><span class="line"><span class="cl"> <span class="k">return</span> <span class="p">{</span> <span class="nx">bucketName</span>: <span class="kt">bucket.id</span> <span class="p">};</span>
</span></span><span class="line"><span class="cl"><span class="p">};</span>
</span></span><span class="line"><span class="cl">
</span></span><span class="line"><span class="cl"><span class="c1">// ESM (used with Bun)
</span></span></span><span class="line"><span class="cl"><span class="kr">export</span> <span class="k">default</span> <span class="kr">async</span> <span class="p">()</span> <span class="o">=&gt;</span> <span class="p">{</span>
</span></span><span class="line"><span class="cl"> <span class="kr">const</span> <span class="nx">bucket</span> <span class="o">=</span> <span class="k">new</span> <span class="nx">aws</span><span class="p">.</span><span class="nx">s3</span><span class="p">.</span><span class="nx">BucketV2</span><span class="p">(</span><span class="s2">"my-bucket"</span><span class="p">);</span>
</span></span><span class="line"><span class="cl"> <span class="k">return</span> <span class="p">{</span> <span class="nx">bucketName</span>: <span class="kt">bucket.id</span> <span class="p">};</span>
</span></span><span class="line"><span class="cl"><span class="p">};</span>
</span></span></code></pre></div><h3 id="4-update-the-pulumi-sdk">4. Update the Pulumi SDK</h3>
<p>Make sure you&rsquo;re running <code>@pulumi/pulumi</code> version 3.226.0 or later:</p>
<div class="highlight"><pre class="chroma" tabindex="0"><code class="language-bash"><span class="line"><span class="cl">bun add @pulumi/pulumi@latest
</span></span></code></pre></div><h3 id="5-install-dependencies-and-deploy">5. Install dependencies and deploy</h3>
<div class="highlight"><pre class="chroma" tabindex="0"><code class="language-bash"><span class="line"><span class="cl">pulumi install
</span></span><span class="line"><span class="cl">pulumi up
</span></span></code></pre></div><h2 id="bun-as-runtime-vs-bun-as-package-manager">Bun as runtime vs. Bun as package manager</h2>
<p>With this release, there are now two ways to use Bun with Pulumi:</p>
<table>
<thead>
<tr>
<th>Configuration</th>
<th>Bun&rsquo;s role</th>
<th>Node.js required?</th>
</tr>
</thead>
<tbody>
<tr>
<td><code>runtime: bun</code></td>
<td>Runs your program and manages packages</td>
<td>No</td>
</tr>
<tr>
<td><code>runtime: { name: nodejs, options: { packagemanager: bun } }</code></td>
<td>Manages packages only</td>
<td>Yes</td>
</tr>
</tbody>
</table>
<p>Use <code>runtime: bun</code> for the full Bun experience. The package-manager-only mode is still available for projects that need Node.js-specific features like function serialization.</p>
<h2 id="known-limitations">Known limitations</h2>
<p>The following Pulumi features are not currently supported when using the Bun runtime:</p>
<div class="note note-warning">
<div class="icon-and-line">
<i class="fas fa-exclamation-triangle"></i>
<div class="line"></div>
</div>
<div class="content"><ul>
<li><strong><a href="https://www.pulumi.com/docs/iac/clouds/aws/guides/lambda/">Callback functions (magic lambdas)</a></strong> are not supported. APIs like <code>aws.lambda.CallbackFunction</code> and event handler shortcuts (e.g., <code>bucket.onObjectCreated</code>) use <a href="https://www.pulumi.com/docs/iac/concepts/functions/function-serialization/">function serialization</a> which requires Node.js <code>v8</code> and <code>inspector</code> modules that are only partially supported in Bun.</li>
<li><strong><a href="https://www.pulumi.com/docs/iac/concepts/providers/dynamic-providers/">Dynamic providers</a></strong> are not supported. Dynamic providers (<code>pulumi.dynamic.Resource</code>) similarly rely on <a href="https://www.pulumi.com/docs/iac/concepts/functions/function-serialization/">function serialization</a>.</li>
</ul>
</div>
</div>
<p>If your project uses any of these features, continue using <code>runtime: nodejs</code>. You can still benefit from Bun&rsquo;s fast package management by setting <code>packagemanager: bun</code> in your runtime options.</p>
<h2 id="start-using-bun-with-pulumi">Start using Bun with Pulumi</h2>
<p>Bun runtime support is available now in <a href="https://www.pulumi.com/docs/iac/download-install/">Pulumi 3.227.0</a>. To get started:</p>
<ul>
<li>Create a new project: <code>pulumi new bun</code></li>
<li>Read the docs: <a href="https://www.pulumi.com/docs/iac/languages-sdks/javascript/">TypeScript (Node.js) SDK</a></li>
<li>Report issues or share feedback on <a href="https://github.com/pulumi/pulumi/issues">GitHub</a> or in the <a href="https://slack.pulumi.com">Pulumi Community Slack</a></li>
</ul>
<p>Thank you to everyone who upvoted, commented on, and contributed to <a href="https://github.com/pulumi/pulumi/issues/13904">the original feature request</a>. Your feedback helped shape this feature, and we&rsquo;d love to hear how it works for you.</p>
