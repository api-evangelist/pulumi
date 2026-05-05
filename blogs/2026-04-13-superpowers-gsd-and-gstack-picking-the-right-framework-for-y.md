---
title: "Superpowers, GSD, and GSTACK: Picking the Right Framework for Your Coding Agent"
url: "https://www.pulumi.com/blog/claude-code-orchestration-frameworks/"
date: "Mon, 13 Apr 2026 00:00:00 +0000"
author: "Engin Diri"
feed_url: "https://www.pulumi.com/blog/rss.xml"
---
<img src="https://www.pulumi.com/blog/claude-code-orchestration-frameworks/meta.png" />
<p>Three community frameworks have emerged that fix the specific ways AI coding agents break down on real projects. <a href="https://github.com/obra/superpowers">Superpowers</a> enforces test-driven development. <a href="https://github.com/gsd-build/get-shit-done">GSD</a> prevents context rot. <a href="https://github.com/garrytan/gstack">GSTACK</a> adds role-based governance. All three started with Claude Code but now work across Cursor, Codex, Windsurf, Gemini CLI, and more.</p>
<p>Pulumi uses general-purpose programming languages to define infrastructure. TypeScript, Python, Go, C#, Java. Every framework that makes AI agents write better TypeScript also makes your <code>pulumi up</code> better. After spending a few weeks with each one, I have opinions about when to use which.</p>
<h2 id="the-problem-all-three-frameworks-solve">The problem all three frameworks solve</h2>
<p>AI coding agents are impressive for the first 30 minutes. Then things go sideways. The patterns are predictable enough that three separate teams independently built frameworks to fix them.</p>
<p><strong>Context rot.</strong> Every LLM has a context window. As that window fills up, earlier instructions fade. You start a session asking for an <a href="https://www.pulumi.com/docs/iac/clouds/aws/guides/">S3 bucket</a> with AES-256 encryption, proper ACLs, and access logging. Two hours and 200K tokens later, the agent creates a new bucket with none of those requirements. The context window got crowded and your original instructions lost weight.</p>
<p><strong>No test discipline.</strong> Agents write code that looks plausible. Plausible code compiles. Plausible code even runs, for a while. But plausible code without tests is a liability. The agent adds a feature and quietly breaks two others because nothing verified the existing behavior was preserved.</p>
<p><strong>Scope drift.</strong> You ask for a <a href="https://www.pulumi.com/docs/iac/clouds/aws/guides/vpc/">VPC with three subnets</a>. The agent decides you also need a NAT gateway, a transit gateway, a VPN endpoint, and a custom DNS resolver. Helpful in theory. In practice, you now have infrastructure you never requested and barely understand. You will also pay for it monthly.</p>
<p>These problems are not specific to Claude Code or any particular agent. They happen with Cursor, Codex, Windsurf, and every other LLM-powered coding tool. The context window does not care which brand name is on the wrapper.</p>
<h2 id="superpowers-the-test-driven-discipline-enforcer">Superpowers: the test-driven discipline enforcer</h2>
<p><a href="https://github.com/obra/superpowers">Superpowers</a> was created by <a href="https://www.linkedin.com/in/jessevincent/">Jesse Vincent</a> and has accumulated over 149K GitHub stars. The core idea is simple: no production code gets written without a failing test first.</p>
<p>The framework enforces a 7-phase workflow. Brainstorm the approach. Write a spec. Create a plan. Write failing tests (TDD). Spin up subagents to implement. Review. Finalize. Every phase has gates. You cannot skip ahead. The iron law is that production code only exists to make a failing test pass.</p>
<p>This sounds rigid. It is. That is the point.</p>
<p>Superpowers includes a Visual Companion for design decisions, which helps when you are making architectural choices that need visual reasoning. The main orchestrator manages the entire workflow from a single context window, delegating implementation work to subagents that run in isolation.</p>
<p>The tradeoff is that the mega-orchestrator pattern means the orchestrator itself can hit context limits on very long sessions. One big brain coordinating everything works well until the big brain fills up. For most projects, this is not an issue. For marathon sessions with dozens of files, keep it in mind.</p>
<p>The workflow breaks down into skills that trigger automatically:</p>
<table>
<thead>
<tr>
<th>Skill</th>
<th>Phase</th>
<th>What it does</th>
</tr>
</thead>
<tbody>
<tr>
<td><code>brainstorming</code></td>
<td>Design</td>
<td>Refines rough ideas through Socratic questions, saves design doc</td>
</tr>
<tr>
<td><code>writing-plans</code></td>
<td>Planning</td>
<td>Breaks work into 2-5 minute tasks with exact file paths and code</td>
</tr>
<tr>
<td><code>test-driven-development</code></td>
<td>Implementation</td>
<td>RED-GREEN-REFACTOR: failing test first, minimal code, commit</td>
</tr>
<tr>
<td><code>subagent-driven-development</code></td>
<td>Implementation</td>
<td>Dispatches fresh subagent per task with two-stage review</td>
</tr>
<tr>
<td><code>requesting-code-review</code></td>
<td>Review</td>
<td>Reviews against plan, blocks progress on critical issues</td>
</tr>
<tr>
<td><code>finishing-a-development-branch</code></td>
<td>Finalize</td>
<td>Verifies tests pass, presents merge/PR/keep/discard options</td>
</tr>
</tbody>
</table>
<p>The results speak for themselves. The <a href="https://github.com/chardet/chardet">chardet</a> maintainer used Superpowers to rewrite chardet v7.0.0 from scratch, achieving a 41x performance improvement. Not a 41% improvement. 41 times faster. That is what happens when every code change has to pass a test: the agent optimizes aggressively because it has a safety net.</p>
<p>Superpowers works with Claude Code, Cursor, Codex, OpenCode, GitHub Copilot CLI, and Gemini CLI.</p>
<h2 id="gsd-preventing-context-rot-before-it-ruins-your-project">GSD: preventing context rot before it ruins your project</h2>
<p><a href="https://github.com/gsd-build/get-shit-done">GSD</a> (Get Shit Done) was created by <a href="https://www.linkedin.com/in/lexchristopherson/">Lex Christopherson</a> and has over 51K stars. Where Superpowers focuses on test discipline, GSD attacks the context window problem directly.</p>
<p>The key architectural decision: GSD does not use a single mega-orchestrator. Instead, it assigns a separate orchestrator to each phase of work. Each orchestrator stays under 50% of its context capacity. When a phase completes, the orchestrator writes its state to disk as Markdown files, then a fresh orchestrator picks up where the last one left off.</p>
<p>Think about why this matters. With a single orchestrator, your 200K token context window is a shared resource. Instructions from hour one compete with code from hour three. GSD sidesteps this entirely. Every phase starts with a full context budget because the previous phase&rsquo;s orchestrator handed off cleanly and shut down.</p>
<p>The state files use XML-formatted instructions because (it turns out) LLMs parse structured XML more reliably than freeform Markdown. GSD also includes quality gates that detect schema drift and scope reduction. If the agent starts cutting corners or wandering from the plan, the gates catch it.</p>
<p>GSD evolved from v1 (pure Markdown configuration) to v2 (TypeScript SDK), which tells you something about the level of engineering behind it. The v2 SDK gives you programmatic control over orchestration, not just static instruction files.</p>
<p>The tradeoff: GSD has more ceremony than the other two frameworks. For a quick script or a single-file change, the phase-based workflow is overkill. GSD earns its keep on projects that span multiple files, multiple sessions, or multiple days.</p>
<p>The core commands map to a phase-based workflow:</p>
<table>
<thead>
<tr>
<th>Command</th>
<th>What it does</th>
</tr>
</thead>
<tbody>
<tr>
<td><code>/gsd-new-project</code></td>
<td>Full initialization: questions, research, requirements, roadmap</td>
</tr>
<tr>
<td><code>/gsd-discuss-phase</code></td>
<td>Capture implementation decisions before planning starts</td>
</tr>
<tr>
<td><code>/gsd-plan-phase</code></td>
<td>Research, plan, and verify for a single phase</td>
</tr>
<tr>
<td><code>/gsd-execute-phase</code></td>
<td>Execute all plans in parallel waves, verify when complete</td>
</tr>
<tr>
<td><code>/gsd-verify-work</code></td>
<td>Manual user acceptance testing</td>
</tr>
<tr>
<td><code>/gsd-ship</code></td>
<td>Create PR from verified phase work with auto-generated body</td>
</tr>
<tr>
<td><code>/gsd-fast</code></td>
<td>Inline trivial tasks, skips planning entirely</td>
</tr>
</tbody>
</table>
<p>GSD supports the widest range of agents: 14 and counting. Claude Code, Cursor, Windsurf, Codex, Copilot, Gemini CLI, Cline, Augment, Trae, Qwen Code, and more.</p>
<h2 id="gstack-when-you-need-a-whole-team-not-just-an-engineer">GSTACK: when you need a whole team, not just an engineer</h2>
<p><a href="https://github.com/garrytan/gstack">GSTACK</a> was created by <a href="https://www.linkedin.com/in/garrytan/">Garry Tan</a> (CEO of Y Combinator) and has over 71K stars. It takes a fundamentally different approach from the other two frameworks.</p>
<p>Instead of disciplining a single agent, GSTACK models a 23-person team. CEO, product manager, QA lead, engineer, designer, security reviewer. Each role has its own responsibilities, its own constraints, and its own slice of the problem.</p>
<p>The framework enforces five layers of constraint. Role focus keeps each specialist in their lane. Data flow controls what information passes between roles. Quality control gates ensure standards at handoff points. The &ldquo;boil the lake&rdquo; principle means each role finishes what it can do perfectly and skips what it cannot, rather than producing mediocre work across everything. And the simplicity layer pushes back against unnecessary complexity.</p>
<p>The role isolation is what makes GSTACK distinctive. The engineer role does not see the product roadmap. The QA role does not see the implementation details. Each role only receives the context it needs to do its job. This is not just about efficiency. It prevents the kind of scope creep where an agent that knows everything tries to do everything.</p>
<p>&ldquo;Boil the lake&rdquo; is my favorite principle across all three frameworks. It is the opposite of how most agents work. Agents default to attempting everything and producing something mediocre. GSTACK says: do fewer things, but do them right.</p>
<p>The tradeoff: 23 specialist roles feels heavy for pure infrastructure work. If you are writing Pulumi programs and deploying cloud resources with <a href="https://www.pulumi.com/docs/iac/concepts/components/">component resources</a>, you probably do not need a product manager role or a designer role. GSTACK shines when you are building a product, not just provisioning infrastructure.</p>
<p>Each slash command activates a different specialist:</p>
<table>
<thead>
<tr>
<th>Command</th>
<th>Role</th>
<th>What it does</th>
</tr>
</thead>
<tbody>
<tr>
<td><code>/office-hours</code></td>
<td>YC partner</td>
<td>Six forcing questions that reframe your product before you write code</td>
</tr>
<tr>
<td><code>/plan-ceo-review</code></td>
<td>CEO</td>
<td>Four modes: expand scope, selective expand, hold, reduce</td>
</tr>
<tr>
<td><code>/plan-eng-review</code></td>
<td>Engineering manager</td>
<td>Lock architecture, map data flow, list edge cases</td>
</tr>
<tr>
<td><code>/review</code></td>
<td>Staff engineer</td>
<td>Find bugs that pass CI but break in production, auto-fix the obvious ones</td>
</tr>
<tr>
<td><code>/qa</code></td>
<td>QA lead</td>
<td>Real Playwright browser testing, not simulated</td>
</tr>
<tr>
<td><code>/ship</code></td>
<td>Release engineer</td>
<td>One-command deploy with coverage audit</td>
</tr>
<tr>
<td><code>/cso</code></td>
<td>Security officer</td>
<td>OWASP and STRIDE security audits</td>
</tr>
</tbody>
</table>
<p>GSTACK works with Claude Code, Codex CLI, OpenCode, Cursor, Factory Droid, Slate, and Kiro.</p>
<h2 id="where-each-framework-fits">Where each framework fits</h2>
<table>
<thead>
<tr>
<th></th>
<th>Superpowers</th>
<th>GSD</th>
<th>GSTACK</th>
</tr>
</thead>
<tbody>
<tr>
<td>What it locks down</td>
<td>The dev process itself</td>
<td>The execution environment</td>
<td>Who decides what</td>
</tr>
<tr>
<td>Orchestration</td>
<td>Single orchestrator</td>
<td>Per-phase orchestrators</td>
<td>23 specialist roles</td>
</tr>
<tr>
<td>Context management</td>
<td>One window</td>
<td>State-to-disk, fresh per phase</td>
<td>Role-scoped handoffs</td>
</tr>
<tr>
<td>Where it shines</td>
<td>TDD, subagent delegation, disciplined plan execution</td>
<td>Marathon sessions, parallel workstreams, crash recovery</td>
<td>Product strategy, multi-perspective review, real browser QA</td>
</tr>
<tr>
<td>Where it struggles</td>
<td>Anything beyond the build phase</td>
<td>Overkill for small tasks, no role separation</td>
<td>The actual writing-code part</td>
</tr>
<tr>
<td>Best for</td>
<td>Solo devs who need test discipline</td>
<td>Complex projects that span days or weeks</td>
<td>Founder-engineers shipping a product</td>
</tr>
<tr>
<td>GitHub stars</td>
<td>149K</td>
<td>51K</td>
<td>71K</td>
</tr>
<tr>
<td>Agent support</td>
<td>6 agents</td>
<td>14+ agents</td>
<td>7 agents</td>
</tr>
</tbody>
</table>
<p>For infrastructure work, GSD&rsquo;s context management matters most. Long Pulumi sessions that provision dozens of resources across multiple stacks are exactly the scenario where context rot bites hardest. GSD&rsquo;s phase-based approach keeps each orchestrator fresh.</p>
<p>Superpowers&rsquo; TDD workflow maps well to application code where unit tests are straightforward. Infrastructure testing is different. You cannot unit test whether an <a href="https://www.pulumi.com/docs/iac/clouds/aws/guides/iam/">IAM policy</a> actually grants the right permissions. You can test the shape of the policy with <a href="https://www.pulumi.com/docs/iac/guides/testing/">Pulumi&rsquo;s testing frameworks</a>, but the real validation happens at <a href="https://www.pulumi.com/docs/iac/cli/commands/pulumi_preview/"><code>pulumi preview</code></a> and <a href="https://www.pulumi.com/docs/iac/cli/commands/pulumi_up/"><code>pulumi up</code></a>. Superpowers still helps here (discipline is discipline), but the TDD cycle is less natural for infra than for app code.</p>
<p>GSTACK shines when the project has product dimensions. If you are building a SaaS platform where the infrastructure serves a product vision, GSTACK&rsquo;s multi-role governance keeps the product thinking connected to the engineering work. For pure infra provisioning, the extra roles add overhead without much benefit.</p>
<p>My honest take: none of these is universally best. Knowing your failure mode is the real decision.</p>
<table>
<thead>
<tr>
<th>What keeps going wrong</th>
<th>Try this</th>
<th>The reason</th>
</tr>
</thead>
<tbody>
<tr>
<td>Code works today, breaks tomorrow</td>
<td>Superpowers</td>
<td>Forces every change through a failing test first</td>
</tr>
<tr>
<td>Quality drops after the first hour</td>
<td>GSD</td>
<td>Fresh context per phase, nothing carries over</td>
</tr>
<tr>
<td>You ship features nobody asked for</td>
<td>GSTACK</td>
<td>Product review before engineering starts</td>
</tr>
<tr>
<td>All of the above</td>
<td>GSTACK for direction, bolt on Superpowers TDD</td>
<td>No single framework covers everything yet</td>
</tr>
</tbody>
</table>
<h2 id="combining-frameworks-with-pulumi-workflows">Combining frameworks with Pulumi workflows</h2>
<p>These frameworks solve the &ldquo;how&rdquo; of agent orchestration. <a href="https://www.pulumi.com/blog/top-8-claude-skills-devops-2026/">Skills</a> (like the ones from <a href="https://github.com/pulumi/agent-skills">Pulumi Agent Skills</a>) solve the &ldquo;what,&rdquo; teaching agents the right patterns for specific technologies. Frameworks and skills complement each other. A skill tells the agent to use <a href="https://www.pulumi.com/docs/esc/guides/configuring-oidc/aws/">OIDC</a> instead of hardcoded credentials. A framework makes sure the agent still remembers that instruction 200K tokens later.</p>
<p>GSD&rsquo;s state-to-disk approach pairs naturally with <a href="https://www.pulumi.com/docs/iac/concepts/inputs-outputs/">Pulumi stack outputs</a>. Each phase can read the previous phase&rsquo;s stack outputs from the state files, so a networking phase can provision a VPC and the compute phase can reference the subnet IDs without any context window gymnastics.</p>
<p>Superpowers&rsquo; TDD cycle maps to infrastructure validation. Write a failing test (the expected shape of your infrastructure). Run <a href="https://www.pulumi.com/docs/iac/cli/commands/pulumi_preview/"><code>pulumi preview</code></a> (red, the resources do not exist yet). Run <a href="https://www.pulumi.com/docs/iac/cli/commands/pulumi_up/"><code>pulumi up</code></a> (green, the infrastructure matches the test). This is not a perfect analogy since infrastructure tests are broader than unit tests, but the discipline of &ldquo;verify before moving on&rdquo; translates directly.</p>
<p>You do not have to pick one framework and commit forever. Try GSD for a long multi-stack project. Try Superpowers for a focused library. See which failure mode bites you most and let that guide your choice.</p>
<h2 id="getting-started">Getting started</h2>
<a class="github-card" href="https://github.com/obra/superpowers" rel="noopener noreferrer" target="_blank">
<img alt="GitHub repository: obra/superpowers" class="github-card-image" src="https://opengraph.githubassets.com/1/obra/superpowers" />
<div class="github-card-content">
<div class="github-card-domain">
<i class="fab fa-github github-card-icon"></i>
github.com/obra/superpowers
</div>
</div>
</a>
<a class="github-card" href="https://github.com/gsd-build/get-shit-done" rel="noopener noreferrer" target="_blank">
<img alt="GitHub repository: gsd-build/get-shit-done" class="github-card-image" src="https://opengraph.githubassets.com/1/gsd-build/get-shit-done" />
<div class="github-card-content">
<div class="github-card-domain">
<i class="fab fa-github github-card-icon"></i>
github.com/gsd-build/get-shit-done
</div>
</div>
</a>
<a class="github-card" href="https://github.com/garrytan/gstack" rel="noopener noreferrer" target="_blank">
<img alt="GitHub repository: garrytan/gstack" class="github-card-image" src="https://opengraph.githubassets.com/1/garrytan/gstack" />
<div class="github-card-content">
<div class="github-card-domain">
<i class="fab fa-github github-card-icon"></i>
github.com/garrytan/gstack
</div>
</div>
</a>
<p>All three frameworks support multiple agents. For Claude Code, the install commands are straightforward:</p>
<div class="highlight"><pre class="chroma" tabindex="0"><code class="language-bash"><span class="line"><span class="cl"><span class="c1"># Superpowers</span>
</span></span><span class="line"><span class="cl">/plugin install superpowers@claude-plugins-official
</span></span><span class="line"><span class="cl">
</span></span><span class="line"><span class="cl"><span class="c1"># GSD (the installer asks which agents and whether to install globally or locally)</span>
</span></span><span class="line"><span class="cl">npx get-shit-done-cc@latest
</span></span><span class="line"><span class="cl">
</span></span><span class="line"><span class="cl"><span class="c1"># GSTACK</span>
</span></span><span class="line"><span class="cl">git clone --single-branch --depth <span class="m">1</span> https://github.com/garrytan/gstack.git ~/.claude/skills/gstack <span class="o">&amp;&amp;</span> <span class="nb">cd</span> ~/.claude/skills/gstack <span class="o">&amp;&amp;</span> ./setup
</span></span></code></pre></div><p>Check each repository&rsquo;s README for Cursor, Codex, Windsurf, and other agents.</p>
<p>If you want a managed experience that handles orchestration for you, <a href="https://www.pulumi.com/product/neo/">Pulumi Neo</a> is <a href="https://www.pulumi.com/blog/grounded-ai-why-neo-knows-your-infrastructure/">grounded in your actual infrastructure</a>, not internet patterns. It understands your stacks, your dependencies, and your deployment history. The <a href="https://www.pulumi.com/blog/10-things-you-can-do-with-neo/">10 things you can do with Neo</a> post shows what that looks like in practice.</p>
<p>Pick one and give it a project. You will know within an hour whether it fixes your particular failure mode.</p>
<a class="btn btn-secondary whitespace-nowrap" href="https://www.pulumi.com/docs/get-started/">
Try Pulumi for Free
</a>
