# Using AI Effectively in Legal Practice

## Limitations, Prompting, and Process

Prepared by Nayan Banerjee / Plain Signal Advisory.

## Recommended pre-reading

### Prompting

[Anthropic's prompt engineering best practices](https://claude.com/blog/best-practices-for-prompt-engineering) provides guidance on instructing AI effectively, including breaking complex tasks into sequential steps.

Skim through:

- [The legal “agents” released by Anthropic for Claude](https://github.com/anthropics/claude-for-legal)
- [The legal summarization use case in the Claude Platform Docs](https://platform.claude.com/docs/en/about-claude/use-case-guides/legal-summarization)

Consider why these “agents” were structured in this way, and why Anthropic chose document summarization as a use case to showcase Claude's applicability in law.

### Risks of AI in law

- [The AI Hallucination Cases Database](https://www.damiencharlotin.com/hallucinations/) by Damien Charlotin of HEC Paris, a global tracker of court and tribunal decisions in which a party was found to have relied on AI-hallucinated material. The country filter can be used to review the Indian entries.
- [10 cases that show Indian courts have an AI hallucination problem](https://www.medianama.com/2026/07/223-10-cases-ai-hallucination-cases-in-indian-courts/) by Medianama, which summarizes several decisions listed in the database.

Consider at which point in each matter the fabricated material could have been caught.

### Process thinking

Two short, practical introductions to flowcharts and decision trees:

- [ASQ's flowchart guide](https://asq.org/quality-resources/flowchart), focusing on the basic procedure and the linked flowchart template.
- [Atlassian's decision-tree guide and template](https://www.atlassian.com/software/confluence/templates/decision-tree), on mapping decision points.

Consider whether you use flowcharts and decision trees in your work, and why or why not.

## Overview

This overview lays the foundation for using AI tools in legal work. It is organised in three connected parts, with room for questions and discussion throughout:

1. **How AI works and where it fails.** We begin with the mechanics: what a large language model does when it answers a question, and why certain failure modes follow directly from that. Examples include invented citations, confidently outdated law, degrading quality in long conversations, and output that never quite matches your voice or format. The objective is to help you anticipate, work around, or manage these failures, especially because being surprised by them may have professional consequences.
2. **Working with AI as a discipline.** The quality of AI output is determined largely by what you give it and how you manage it. We walk through a working method in three phases: what to set up before any prompting begins; how to instruct the AI within a task; and how to manage conversations over time so that quality does not silently degrade. Each technique follows directly from a limitation covered in the first part.
3. **Knowing your own process.** The working method above assumes you know which stage of a task you are at. We therefore also consider how to think about a process: what a workflow is, how to recognise the ones you already use every day, and how tools as simple as a flowchart or a decision tree make them visible. The objective is to help you decide where AI can be applied and where professional judgment must remain.

## Part 1 — How AI works and where it fails

### Prediction, not knowledge

A large language model does not look up answers. It generates text by predicting, word by word, what is most likely to come next given everything it has seen in training and everything in your prompt.[1] This is why its output is so fluent. But fluency and confidence are properties of the writing style, not indicators that the content is correct. A model states a wrong answer in exactly the same assured tone as a right one.

### Hallucination is structural

When a model does not have strong grounding for an answer, it does not stop — it fills the gap with something plausible. This is called hallucination, and it is a direct consequence of how the technology generates text.[1] Stanford's RegLab research on dedicated legal research AI found that even purpose-built legal tools produce incorrect or unsupported answers at material rates.[2] AI output is a draft to be verified, never a source to be relied upon.

### Retrieval is not reading

When an AI tool works across a set of documents, it typically does not read every page every time. It retrieves what its search layer judges relevant — extracts, chunks, or in some cases summaries and metadata — and answers from that.[2] For legal work this matters enormously. In a 300-page transaction document set, the answer to your question may sit in a single carve-out to an indemnity clause. If the retrieval step never surfaces that clause, the AI will still answer your question — confidently, and from an incomplete picture.

### It does not know what it does not know

Unless specifically instructed otherwise, an AI model may not respond with “I don't know” or “that is not in these documents.” It answers. Where the material contains no answer, it may construct one.[1]

### It agrees with your framing

This tendency — called sycophancy — has two technical causes. First, the model generates text conditioned on everything in your prompt, and the premise of your question is part of that context: a continuation that agrees with its context is statistically more likely than one that contradicts it, so your framing functions as evidence. Second, these models are refined using human preference ratings, and human raters systematically reward agreeable, confirming answers over challenging ones — deference is trained in.[3]

The result is a hazard tailored precisely to lawyers: the profession trains you to embed the desired answer in the question, and a leading question is the single worst way to interrogate an AI. It will find your answer for you whether or not the documents support it. This has been measured in legal settings specifically: a systematic study found that leading models routinely failed to correct questions built on false legal premises, answering as though the premise were true.[4] The disciplined alternative is covered in Part 2: ask neutrally, never disclose the answer you are hoping for, and seek the contrary reading separately.

### Its legal knowledge is frozen — and foreign-weighted

A model's built-in knowledge dates from a training cutoff, months or more in the past; it does not know the amendment notified last quarter or the judgment delivered last month unless the tool retrieves them live. Less obviously, that built-in knowledge is heavily weighted toward the jurisdictions that dominate its training data — overwhelmingly the United States and United Kingdom.[5] Asked to recall Indian law from memory, a model may state a superseded position, or quietly blend in a foreign doctrine that reads naturally and is wrong here.

It is important to understand the underlying data sources for the AI model and whether the data sources are updated in real time. You may need to supply the authority — the section, the judgment, the circular — in the prompt, and instruct it to work from what you have provided.

### It has no memory — only a context window

It is natural to assume the AI “remembers” your conversation the way a colleague would, especially since it clearly arrives with knowledge and instructions already built in. The reality is different, and understanding it explains most of the quality problems users experience.

What the model “knows” permanently lives in its weights — the parameters fixed during training. That is where its language ability and general knowledge reside. Weights are frozen. Everything else — your instructions, your documents, every message you have sent, every response it has given including drafts you rejected, plus the tool's own standing instructions — lives in the context window: a finite working space.

The model has no memory of your conversation at all. Every time it responds, it re-reads the entire transcript from the beginning and generates from that. The transcript is the memory. There is no separate store where important things are kept and trivialities discarded — it is all flat text, competing for attention. That competition is where long or multi-layered complex conversations degrade the quality of responses, through three mechanisms.

First, attention dilution: to generate each word, the model spreads a finite attention budget across everything in the context; the longer the thread, the less weight any single instruction receives — your careful direction from an hour ago is now one voice among thousands. Second, positional weakness: models demonstrably attend best to the beginning and end of their context and worst to the middle,[6] which is exactly where mid-conversation instructions end up as the thread grows. Third, contradiction accumulation: superseded drafts and corrected errors are never deleted from the transcript; the model sees draft one, your correction, draft two, another correction — and must infer statistically which is current. Sometimes it regresses to the version you rejected, because that text is still sitting there, still influencing prediction. And when a thread finally exceeds the window altogether, the oldest material is truncated or crudely compressed.

The symptoms are recognisable once you know to look: the AI reintroduces an error you corrected, drifts from instructions it followed earlier, or blends the current task with a previous one. Part 2 covers the working practices that prevent this.

### It predicts text — it does not manage documents, and it does not know your voice

Two related frustrations stem from expecting something the technology does not do.

**On format:** a model generates text one word at a time, left to right. It has no document model behind that stream — no layout engine, no running register of clause numbers, no table of defined terms, no awareness of pagination. When it “maintains” a numbering scheme across forty paragraphs, it is predicting that scheme afresh at every step, and prediction consistency degrades over length.[7] So prose and simple structure are reliable; precise sustained formatting — clause cross-references, defined-term consistency across a long instrument, exact house style — is unreliable by construction, not by defect. Tools that embed the model inside document software close part of this gap, because the software manages the format while the model supplies the words.

**On voice:** a model can imitate a famous author because that author's entire published corpus — plus decades of commentary and imitation — is in its training data; the style is baked into the weights. Your voice is not. The model has never seen it, and cannot recall what it has never seen. It can approximate your style within a working context if you show it examples — a technique covered in Part 2 — but the approximation is shallow: it captures surface features like sentence length, formality, and characteristic phrasing far better than deep ones, like your judgment about what to leave out. And the mimicry itself fades over a long thread, for exactly the memory reasons above.

The reframe that dissolves the frustration: the tool produces the substance of a draft, not the deliverable. Voice and format finishing is human work.

### What this means for you professionally

This is no longer a theoretical risk, and it is no longer a foreign one. A global database maintained at HEC Paris tracks court and tribunal decisions worldwide in which a party was found to have relied on AI-hallucinated material — growing at several new cases per day, with consequences ranging from costs and fines to suspensions from practice.[8]

India is firmly on that map. In July 2026, the Supreme Court in *Pooja Ramesh Singh v. Jammu and Kashmir Bank Ltd.* (2026 INSC 668) set aside NCLT and NCLAT orders that rested on AI-fabricated precedents, holding that a decision tainted by hallucinated material is no decision in the eyes of the law, that citing unverified AI-generated judgments is misconduct on the part of an advocate, and directing the Bar Council of India to frame disciplinary norms.[9] The Bombay High Court imposed ₹50,000 in costs in January 2026 on a party whose written submissions contained fake case law bearing the telltale formatting of raw AI output; a Delhi High Court petition was withdrawn in September 2025 after opposing counsel exposed its citations as fabricated; the Income Tax Appellate Tribunal recalled one of its own orders after discovering it relied on fictitious case law; and the Supreme Court's November 2025 White Paper on AI and the judiciary documented hallucinated precedents passing undetected through entire tiers of appeal.[10] The Court's draft AI rules, released in June 2026, would require lawyers to disclose their use of AI.[10]

Three things follow for daily practice:

1. Verification of every AI-assisted output is now a matter of professional conduct. Unverified reliance is being framed as misconduct.
2. Exposure runs in both directions: opposing counsel are increasingly the ones who find the fabricated citation, which means your unverified filing is their opportunity.
3. The AI is a tool in your workflow, but you hold the liability for its output.

## Part 2 — Working with AI as a professional discipline

This part is organised the way the work itself is organised: what you set up once for a matter, before any prompting begins; how you instruct the AI within a task; and how you manage conversations over time so that quality does not silently degrade.

### Setting up: before the work begins

**Give the matter a home.** Both Claude and Harvey allow you to create a persistent workspace for a matter — in Claude these are called Projects.[11] A project holds standing instructions and key documents, and every new conversation started inside it begins with all of that context already loaded. This is the direct answer to the memory limitation in Part 1: material placed in a project is re-supplied fresh at the start of every chat, at the position where the model attends best — it never suffers the mid-thread burial that degrades instructions given in conversation.

**Write a matter instruction document — once.** The core of the project is a standing document, written at the start of a sustained piece of work, that sets out:

- the matter context and parties;
- defined terms;
- which documents are the authoritative sources;
- the required citation format;
- the expected tone and audience of outputs; and
- the standing rules: quote operative language verbatim with its location, flag anything unsourced, and state clearly when an answer is not found in the materials.

Written once, it governs every conversation on the matter, produces consistency across everyone on the team, and removes the daily burden of re-explaining context. It is also — worth noticing — the first piece of your own process you will ever have written down.

**Supply precedents and exemplars.** The most effective way to control style and structure is the way lawyers have always worked: from precedent. Place in the project three to five examples of what good looks like — the firm's model narrative, a well-drafted clause of the relevant kind, a memorandum in the house style — and instruct the AI to match them. This is far more powerful than describing the style in words but is no replacement for deep judgment or your voice.[12]

**Set the role, the reader, and the constraints.** Tell the AI who it is acting as, who will read the output, and what rules apply. For example:

> You are assisting a corporate associate preparing a first-draft summary of change-of-control provisions for review by a partner. Use defined terms from the SPA. Flag ambiguity rather than resolving it.

The difference between a poor output and a strong one is usually the difference between a one-line request and an engineered instruction.[12]

### Instructing: within a task

**One task per prompt — never mix research with drafting.** Research produces claims that must be verified against authority. Drafting produces language built on material already verified. When a single prompt asks for both — “find the relevant case law and write the argument section” — the output blends what was found with what was generated, and you can no longer separate the two. The disciplined sequence is to prompt for research, verify the results yourself against the actual sources, and only then prompt for drafting, supplying verified material as the input.

**Direct it to the document, not the summary.** Because retrieval works from extracts and summaries, questions put generally to a large document set invite summary-level answers. Counter this by directing the AI to the specific documents and sections that matter, and by requiring it to quote the operative language verbatim with its location — document name, clause, page. An answer that includes “Clause 14.3(b) of the SPA states: ‘…’” can be checked in seconds. An answer that paraphrases “the agreement provides for a cap on indemnity claims” can conceal exactly the carve-out you needed to find. If you cannot trace an answer to quoted language in a specific location, treat it as unverified.

**Specify the citations you require.** “Include citations” is not an instruction; it is an invitation to decorate. Specify:

- citations must be pinpoint;
- they must be to the documents provided, or, for legal research, to real and verifiable authority you have supplied;
- they must follow the firm's format; and
- any proposition the AI cannot source must be expressly flagged as unsourced.

This turns citations into an audit trail. Recall from Part 1 that the model's own recalled law may be stale or foreign-blended.

**Instruct it on ignorance.** For example:

> If the answer is not contained in the materials provided, state that clearly. Do not infer, estimate, or fill gaps.

**Ask — never lead.** The model treats your framing as evidence and is trained toward agreement. Consider framing the prompt not as “confirm that clause 12 permits assignment without consent” but “what does clause 12 provide regarding assignment?”

**Do not disclose the answer you are hoping for when asking the AI to verify something.** You can also run the question from the other side as a separate prompt, for example:

> What is the strongest reading of clause 12 against assignment without consent?

If the two answers collide, you have found exactly the issue that needed your judgment.

**Specify the format — with realistic expectations.** After the document has been drafted and the content verified, rework the draft after explicitly stating the structure, length, and format you want. Remember that asking the AI to multi-task, such as drafting and formatting, can lead to drift. Formatting is ideally a post-verification exercise. But calibrate expectations: as Part 1 explained, the model predicts text and does not manage documents. Expect prose and simple structure to hold; expect sustained precise formatting — clause cross-references, defined-term discipline across a long instrument — to drift, and plan for the finishing pass to be manual.

### Managing the conversation: across the session

**One task, one chat.** Start a fresh conversation when you switch tasks. A thread that has handled research, then drafting, then a tangent about a different clause is context polluted for all three — the model is still attending to all of it, whether you are or not.

**Recognise decay, and restart rather than persist.** The symptoms from Part 1 — reintroduced errors, drifting from earlier instructions, blending tasks — are the signal that the context has degraded past usefulness. The instinct is to keep correcting; the discipline is to stop. A fresh chat with a clean statement of the current state outperforms a long thread of accumulated corrections, because it deletes the noise instead of arguing with it.

**Summarise before you leave.** End any substantial working thread with a closing prompt:

> Summarise the decisions made, the current state of the draft, and the open items, in a form a colleague could pick up from cold.

Save that summary into the project — the tool's own documentation confirms that context does not carry across chats within a project unless it is added to the project knowledge.[11] This is the bridge between conversations: the next chat starts with a small, current, high-attention context instead of inheriting a bloated one — which is precisely why it works.

**Fork instead of contaminating.** Editing one of your earlier messages branches the conversation from that point, leaving the original thread intact.[13] Use it to test an alternative — a different drafting approach, a different assumption — without pouring the experiment into the main thread's context.

## Part 3 — Knowing your own process

### The assumption behind everything above

Every practice in Part 2 assumed something: that you know which stage of a task you are at. Research or drafting? Reviewing or summarising? First pass or final check? That awareness is process thinking, and it is the real foundation of effective AI use.

### What a workflow is

A workflow is simply a repeatable sequence of steps with defined inputs, outputs, and decision points. Legal training does not usually involve writing these down — the steps are absorbed through experience and carried out from memory. But they exist, and every associate runs dozens of them.

A research memorandum is a workflow: scope the question, search, read the authorities, synthesise, draft, check citations, submit for review. A disclosure review is a workflow. Preparing billing narratives is a workflow: gather the recorded time of everyone on the matter, allocate it appropriately across partner, senior partner, and associate work, draft narratives in the firm's convention, and review before submission.

The pre-reading introduced the two simplest tools for making a workflow visible. A flowchart records the steps and their sequence — each activity a box, each decision a branch. A decision tree does the same for a single decision point with multiple paths. Write down what you actually do, step by step, at the level of detail you would need to hand the task to someone new.

### Why this matters for AI

You cannot decide where AI belongs in a process you have not made visible. Once a workflow is written down, each step tends to classify itself into one of three categories:

- steps that are repeatable and rule-based, where AI performs well;
- steps where AI can produce a first draft but a human must verify; and
- steps involving professional judgment, allocation of responsibility, or client-facing decisions, which remain human work.

### The habit to build

Before reaching for the AI on any task, ask three questions:

1. What are the steps of this task?
2. Which step am I on right now?
3. Is this a step where AI generates, AI drafts and I verify, or I decide?

## Sources and notes

[1] Adam Tauman Kalai, Ofir Nachum, Santosh Vempala and Edwin Zhang (OpenAI), [Why Language Models Hallucinate](https://openai.com/index/why-language-models-hallucinate/) (2025), arXiv:2509.04664 — frames language models as statistical text generators and shows that generative errors arise naturally from the objectives of language-model training even on clean data; further shows that standard training and evaluation procedures reward confident guessing over acknowledging uncertainty, which is why models answer rather than abstain when they lack grounding.

[2] Stanford HAI / RegLab, [AI on Trial: Legal Models Hallucinate in 1 out of 6 (or More) Benchmarking Queries](https://hai.stanford.edu/news/ai-trial-legal-models-hallucinate-1-out-6-or-more-benchmarking-queries) (2024) — empirical benchmarking finding that dedicated legal research AI tools produce incorrect or unsupported answers at material rates. The tools studied were retrieval-based, demonstrating that a retrieval layer bounds what the model answers from and mitigates but does not eliminate the problem.

[3] Mrinank Sharma et al. (Anthropic), [Towards Understanding Sycophancy in Language Models](https://arxiv.org/abs/2310.13548) (2023) — evidence that state-of-the-art AI assistants consistently exhibit sycophancy across tasks, and that human preference data used in fine-tuning rewards responses matching the user's stated views, incentivising agreement over accuracy.

[4] Matthew Dahl, Varun Magesh, Mirac Suzgun and Daniel E. Ho, [Large Legal Fictions: Profiling Legal Hallucinations in Large Language Models](https://academic.oup.com/jla/article/16/1/64/7695641), *Journal of Legal Analysis* 16(1) 64–93 (2024), doi:10.1093/jla/laae003 — systematic study finding general-purpose models hallucinated on at least 58% of specific, verifiable case-law queries, often failed to correct questions built on false legal premises, and performed unevenly across jurisdictions and court levels.

[5] Kenneth Wehr et al., [Alignment Debt: The Hidden Work of Making AI Usable](https://arxiv.org/abs/2511.09663) (2025), §2.4 — documents that commercial large language models are trained primarily on English-language, Western-centric corpora, with performance patterns across benchmarks reflecting that skew. The application of this general finding to Indian law specifically is the authors' inference, consistent with the jurisdictional variation documented in [4]; readers should treat it as a reasoned extension rather than a directly measured result.

[6] Nelson F. Liu et al., [Lost in the Middle: How Language Models Use Long Contexts](https://arxiv.org/abs/2307.03172), *Transactions of the Association for Computational Linguistics* 12:157–173 (2024) — empirical finding that model performance is highest when relevant information sits at the beginning or end of the input context and degrades significantly when it sits in the middle.

[7] Xiang Liu et al., [LongGenBench: Benchmarking Long-Form Generation in Long Context LLMs](https://arxiv.org/abs/2409.02076) (ICLR 2025) — evaluation of nine advanced models on long-form generation, finding significant performance degradation as output length grows, with failure modes including disregard for instructions, incomplete responses, and repetitive content — supporting the claim that generation consistency, including sustained formatting, degrades over length.

[8] Damien Charlotin (HEC Paris), [AI Hallucination Cases Database](https://www.damiencharlotin.com/hallucinations/) — continuously updated global tracker of court and tribunal decisions involving reliance on AI-hallucinated material; case count as at July 2026. Because the database changes daily, the figure quoted should be re-checked before presentation.

[9] *Pooja Ramesh Singh v. Jammu and Kashmir Bank Ltd. & Anr.*, 2026 INSC 668, Supreme Court of India, judgment dated 2 July 2026 (Narasimha and Aradhe JJ.) — listed in the [Supreme Court's landmark judgment summaries](https://www.sci.gov.in/landmark-judgment-summaries/); reported at [Verdictum](https://www.verdictum.in/supreme-court/pooja-ramesh-singh-v-jammu-and-kashmir-bank-ltd-2026-insc-668-ai-generated-precedents-1616998). The holdings summarised in the text should be verified against the judgment itself before presentation.

[10] Medianama, [10 cases that show Indian courts have an AI hallucination problem](https://www.medianama.com/2026/07/223-10-cases-ai-hallucination-cases-in-indian-courts/) (July 2026) — secondary survey supporting the cluster of Indian instances cited in the text, including the Bombay High Court costs order, Delhi High Court withdrawal, ITAT recall, the Supreme Court's November 2025 White Paper, and the June 2026 draft AI rules.

[11] Anthropic, [What are projects?](https://support.claude.com/en/articles/9517075-what-are-projects) and [How can I create and manage projects?](https://support.claude.com/en/articles/9519177-how-can-i-create-and-manage-projects) — official product documentation confirming that project knowledge and instructions are applied across all chats within a project, and that context does not carry between chats unless added to the project knowledge base. Harvey's equivalent workspace features should be verified against the firm's own configuration.

[12] Anthropic, [Best Practices for Prompt Engineering](https://claude.com/blog/best-practices-for-prompt-engineering) (2026) — the tool-maker's guidance that output quality depends on clear, specific, context-rich instructions, including the use of examples and the decomposition of complex tasks into sequential steps.

[13] Anthropic, Claude Help Center, [Prototype AI-powered apps with Claude Artifacts](https://support.claude.com) — official guidance instructing users to go back to a previous message and click “Edit” to create a new conversation branch, confirming the edit-to-branch behaviour described. A practical walkthrough of the mechanics is available at [Branch conversations in Claude](https://claudetopdf.vercel.app/branch-conversations-in-claude).
