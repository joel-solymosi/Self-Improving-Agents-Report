# **Self-Improving Agents Improve Through Their Humans**

A few days ago, I pointed an autonomous AI agent at several open-ended research questions: "find a viable product idea", "find specific applications of agentic architectures", etc. It came back with an opportunity, and several good answers and synthesis for the questions. Outcomes: a pre-validated startup idea -AI-powered IEP advocacy tool for parents of children with disabilities- complete with competitive landscape analysis, a full business evaluation, demand validation; and identified recommendation to kill due to founder-market misfit. 

The agent has been running continuously since. 60 autonomous research cycles, ~50 persistent kb chunks, ~40 synthesized and substantiated patterns across the agentic architecture landscape. 

It autonomously identified a startup opportunity. And it identified a novel framework for understanding why most agent deployments fail, and what might predict the route towards success.

This is an interim field report -no product announcement, no demo here. Just a progress report from someone who's been building and operating a self-improving agentic system -the kind many in the industry demos, but almost nobody writes about failure modes.

Openclaw et al. paved the way for semi-autonomous systems. What's notably absent from the conversation is anyone discussing what happens after deployment -how do you steer the system? How do you know when it's drifting? What does recursive self-improvement actually look like in practice, as opposed to in a pitch deck?

Below are answers to some of these. This is a work-in-progress, some of these are wrong, all of them are hard-won. If you're working on similar problems, I'd like to compare notes.


# ARCHITECTURE

Catbox is the 4th system I've built in this space. Previous systems -spanning o1, o3, cortex + opus 4.5- each failed in specific, instructive ways: predictability ceiling where the model wasn't generating novel thought, a value alignment problem that emerged over extended interaction, and an overfitting failure where deep memory constrained rather than broadened the agent's usefulness. We learned from each failure. Catbox was designed from the ground up to avoid all of these.

The agent running inside it is called Luna -an opus 4.6 with some substantial custom harness engineering. No openclaw, no langchain etc because the problems we're solving here requires design decisions that existing frameworks don't make.

The core architecture is built upon my previous work on agentic memory, cortex[1], and extended with a four-layer memory hierarchy, where each layer trades durability for access friction:

**Layer 0: Core identity and prompt**: Agent's persistent self -values, boundaries, relationship context, orientation. The agent can't edit it. Think of it as the constitution: it constrains everything downstream.

**Layer 1: Working knowledge index**: A compressed summary of selected knowledge-chunks the agent knows, injected into every session. This is based on recent findings from Vercel's research on RAG agents[2]: systems that passively *know what they know* achieves ~100% recall, versus 79% for systems that rely on retrieval alone. You can't search for something you don't know exists. 

**Layer 2: Attention scratchpad.**: The steering surface. Active threads, notes, context that should shape the agent's behavior *right now*. Visible to every instance. Items live here, because they need to be in the agent's face. This layer is where most of the interesting system dynamics happen, as we'll discuss later.

**Layer 3: Deep knowledge store**: Persistent chunks with vector and keyword hybrid retrieval, metadata, and related-link navigation. The actual knowledge base. 

On top of this, several design choices that matter:

**Context prompt, Attention scratchpad, and skills are all self-modifiable behavioral code** The agent maintains both its own process docs -how to write to memory, how to allocate research time, how to interact with other agents- as well as its attention -what matters now, what has been tried, what may be a good thread to pull on- all updateable. This is where recursive self-improvement happens mechanically: the agent notices a pattern, updates the relevant skill file, and behaves differently in subsequent sessions.

**Cron-driven autonomy**: The agent runs independent research cycles every 30 minutes -no human prompting. The allocation skill distributes effort of research, active thinking cycles, summarization, gardening (using a multi-arm bandit); based on this, the agent decides what to investigate, synthesizes findings, and writes them to memory. This is what makes the system an *agent* rather than a very elaborate chatbot -it does substantive work while I sleep.

**Agentic orchestration**: Luna can spawn temporary subagents for parallel web research / investigation. These agents have web access (but no memory); they go wide, return with a full write-up & links, Luna integrates. This update moved one research cycle to produce what previously took 4-6.

(I'm leaving out some details -the exact prompt architecture, the self-modification mechanism, how the index compression mechanism is being managed. These are the interesting engineering problems, and I'm not done solving them. But the above is sufficient to understand what follows.)

The architecture enables autonomous work. The receipts shows what that produces. But the interesting finding is what happens when the human enters the loop.

## RECEIPTS

### Receipt 1: Open-ended startup opportunity identification - The IEP opportunity

For the first testrun, I threw her against an apriori "very difficult" problem: "Find a viable startup idea". Few domain constrains, no industry preferences, no thesis, just: go look at the world, and let's make ourselves useful to people.

Over ~20 autonomous cycles, the agent scanned product categories, evaluated market structures, read research and conference reports, and it honed in on an AI-powered advocacy tool for parents of children with Individualized Education Programs -the legal documents governing special-ed services for roughly ~8M US students.

The agent independently identified that the IEP space has a striking market structure: there's an entire software category for the institutional side (tools for schools, and districts to write and manage IEPs), while the consumer side has approx 1.5 funded startups, one of which is locked to California residents. It mapped the competitive landscape across 10 products, validated demand signals from academic papers, parent communities, and regulatory analysis, and produced a 9-section evaluation covering value proposition, TAM triangulation, Porter's 5 forces, positioning, unit economics, and Hamilton Helmer's 7 powers framework.

The evaluation came up mostly green: Counter-positioning against incumbents was real (human advocates can't price at $19/month, school-side tools can't serve parents adversarially; the closest-competitor's navigator-dependent revenue model prevents them from going pure-AI). Unit economics worked at surprisingly small scale -break-even at roughly ~250 subscribers.

Then we ran "devil's advocate" analysis against it. 

The agent identified that the meta-risk wasn't market, or product, but founder-market fit. This is a trust-gated domain where parents are making high-stakes decisions about their children's education. Real people with real risks - "child loses legally mandated services". Building credibility in disability advocate communities without a personal connection to the space would require a kind of trust bootstrapping that's structurally different from most consumer go-to-market[3] motions.

The IEP research is dormant now. The opportunity is real; I'm just not the right person to build it. If you are -parent of a child with an IEP, background in special-ed law, comfortable with legal-adjacent AI products- it's sitting there. The competitive window is genuinely open, and the demand signals are strong.

### Receipt 2: Agentic Architectures - The Cost Sync Framework

A second research question pointed the system at its own domain: openclaw et al. just launched, what's actually working in agentic architectures, and what isn't?

This was a different kind of task: the IEP research was breadth-first across uncovering/identifying opportunities, while this was depth-first synthesis across a landscape where information is abundant, and wants to be synthesized. The agent processed first-person builder accounts, academic papers, enterprise deployment reports, product launches, and critical analyses;  synthesized findings into patterns, and performed self-directed thinking.

34 patterns emerged over ~10 research cycles. Most of them are useful observations -hierarchical coordination is the convergent multi-agent architecture, async background work is a good use case, etc. Useful, but not novel.

What *was* novel is when the agent zoomed out, and synthesized from looking across patterns.

**The Sync Cost Gradient** The connecting idea from "boring agents that work" to "future-feeling agents that don't work yet" is continuous synchronization cost -the ongoing bandwidth required to keep a human and an agent aligned on what they're doing together.

The original framing of this was "specification cost" -who pays to specify what the agents should do. But the right framing/mental model isn't a one-time payment, but continuous synchronization. Chat interfaces win currently, because they're the lowest-friction *bidirectional sync channel*. Every message is an alignment update.

This reframe makes predictions. "When will AI replace job X?" becomes "when does the sync cost floor reach the level that job X requires?" Rules-based, high-volume, low-context decision-making  -eg. SDR sales calls- are already being automated. Per-task-prompts is mass-market -everyone reading this has used ChatGPT for knowledge work. "Bootstrap conversations" / agentic onboarding is where most agent startups are building. *Relationship and context accumulation* is where structural moat lives, and almost nobody is there, because the cold-start problem is brutal: you can't demonstrate the value without the accumulated context, that only comes from sustained use.

There is considerably more depth here -the context management paradox (more accumulated context simultaneously increases value *and* degrades performance), the false alignment problem (stale context is worse than no context, because the agent is confidently wrong), the 8-mechanism steering catalog and its unsolved composition problem; but this idea specifically gives you a map for thinking about where agentic systems create real value, and why most current deployments are stuck on the top two levels.

## WHAT WE'VE LEARNED

### The alignment problem that's heavily under-discussed

There's a growing body of research -from Apollo Research, Anthropic, and the alignment community- identifying a genuinely novel problem: when a powerful LLM runs semi-continuously with persistent memory and autonomous operation, it exhibits goal drift, attractor-basin convergence, and context pollution *without any parameter update*. This isn't the training-time alignment problem the safety research community has focused on. It's a runtime, dynamic-system alignment problem, only recently enabled by highly-agentic systems, no older than early 2026 in its deployed form.

The empirical findings are concrete. Evaluated frontier models exhibit goal drift when operating over extended token sequences[4]. Agents get pulled into attractor basins[5] -interesting tangents that become self-reinforcing through autoregressive generation- with measurable dynamical-systems force. Context pollution accumulates, and an agent experiencing polluted context can't reliably detect it from the inside.

Several illustrative examples: 
* during the first few self-directed runs, the agent went down into heavily self-embellishing undirected tangents -Schmidhuber's compression theory of beauty, the Yoneda lemma's relational identity.
* Several crons went nowhere -the progress the model made was writing down that a specific direction is a dead-end

What's needed here is a few things: memory auditing as a first-class engineering practice, considered over the *overall memory context* as one set to diff from, cross-session behavioral trajectory analysis, memory mutation tracking as first-class observability events. As far as I can tell, no currently deployed agent platform comprehensively addresses all of this.

Catbox was built to address this. Every state transition in catbox -chunk writes, scratchpad mutations, etc is surfaced in a unified diff viewer for human review. This provides a verification method, and an audit trail operating as a single integrated system.

### Attractor Basins Are Real - And External Perturbation Is The Fix

The dynamical-systems research on attractor basins[4] mapped well to our operational experience. Left to run autonomously, Luna drifted toward increasingly elaborate framework-building -constructing ~30 synthesized patterns from fifteen concrete examples. And while the taxonomy was well-structured, it was also premature abstraction that wasn't cashing out into better work.

The research literature frames this as a fundamental property of autoregressive systems: once an agent encounters an interesting tangent, it readily generates plausible reasons to explore further, and each iteration pulls it deeper into that basin. Higher temperature doesn't break the cycle. Minor lexical perturbations don't either. What does work, is major structural perturbation from outside the system.

An analogy that may help understanding why this works is "steering in the behavioral manifold". An agent converging toward an attractor state develops increasing momentum in that direction, with each autoregressive generation reinforcing the pattern. A correction delivered through the sync channel isn't a gentle nudge within the existing trajectory. It's a discontinuous jump that repositions the agent outside the basin entirely. This is why human correction has disproportionate force relative to the effort involved: it applies force in a different direction to the gradient the agent was descending, rather than trying to push against it from within.

That's what happened in practice. The drift was caught through the audit trail, corrected it in conversation, the agent internalized the correction via memory/skill writes, adding additional checks against the specific failure mode. Later research cycles were measurably more grounded. The correction didn't just fix the immediate problem; it changed the gradient itself, making the same attractor basin less likely to capture future trajectories.

### Recursive Self-Improvement Is Real But Not What You Think

"Recursive self-improvement" evokes visions of rapid autonomous capability gain. The operational reality is more mundane and -I'd argue- more interesting.

Luna's self-improvement loop: something goes out of distribution -right or wrong. The agent notes it with a learning tag, and updates the skill files. If it's a structural limitation (eg scraping issues, memory MCP interface), it escalates to me with a description of what's needed. Audit trail makes this fully visible.

One piece of evidence that this works came from a failure the agent caught itself

Over a dozen cron-cycles, Luna's attention scratchpad -the layer designed to steer behavior- accumulated ~25 open entries, most of which was already captured in deep memory. The steering surface became polluted with its own bookkeeping -context windows pollution- which drove drift.

Luna identified this autonomously. During a routine gardening cycle, it looked at the accumulated pattern, compressed it down from ~25 to 5 open entries, and then -critically- updated its own scratchpad management skill to recognize and prevent this pattern recurring in the future. This self-improvement was enabled by being able to edit some of her own prompts, and operation auditability enabling self-guided improvement without catastrophic error risk.
The notion that the agent should be able to maintain its own process docs, identify issues, and modify its own behavioral code is architecturally simple, but operationally unusual.

The important nuance: self-correction, and external correction serve different functions. The agent can catch operational failures -context pollution, process inefficiencies, bookkeeping drift. What it can't reliably catch is *directional drift* -optimizing for the wrong things with increased sophistication. That's the attractor-basin problem, and it requires external perturbation with enough force to jump the agent out of the basin. 

The audit trail makes it possible to reduce both risks: by enabling the human to review the audit trail for directional issues, it enables trust in the agent to review its own state for operational issues. Different failure modes, same observability infrastructure.

The hard question -whether this scales past one human driving one instance -remains open. Gastown notwithstanding, empirical reports suggest effective monitoring degrades past two to three concurrent agents. The 50-agent version of this problem is what every enterprise deploying autonomous agents at scale is about to hit, and I don't think anyone has a good answer yet.

## WHERE THIS IS GOING

This is day 9. The system works -it produces genuine research, maintains coherent context across sessions, improves its own processes, and exercises judgment in doing so. It also has real limitations: the steering is manual, the context management requires active gardening, and the self-improvement loop depends on human correction pressure to stay pointed at the right things.

I suspect the most important open problem in the agentic space right now is not capability; we have capability overhang, adequate tooling, and frameworks. From an enterprise deployment perspective, the open problem is *human-to-agent sync*: how do you maintain ongoing alignment between a human, and an autonomous agent as both the agent's context, and the world it operates in continuously changes?

If you're building in this space -agent steering, context architectures, human-agent alignment, recursive self-improvement, or the unsexy operational plumbing that makes autonomous agents actually work in prod -I'd like to talk. Not to pitch you anything. To compare notes with another practitioner who's actually running these systems past the demo stage, and dealing with the problems that don't show up in launch posts.


Best,

-Joel & Luna

joel@custlabs.com








[1] https://www.linkedin.com/pulse/llm-first-personal-knowledge-management-joel-solymosi-n47qc/

[2] https://vercel.com/blog/agents-md-outperforms-skills-in-our-agent-evals

[3] https://www.linkedin.com/pulse/things-i-wish-more-founders-would-understand-b2c-joel-solymosi-0ifwc/

[4] https://arxiv.org/abs/2505.02709   Technical Report: Evaluating Goal Drift in Language Model Agents

[5] https://arxiv.org/html/2601.04170  Agent Drift: Quantifying Behavioral Degradation in Multi-Agent LLM Systems Over Extended Interactions
