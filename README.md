# Meta-Ethical Round Table 🔮

**A multi-model ethical deliberation system that discovers moral consensus under veil of ignorance conditions, rather than encoding it top-down.**

---

## The Problem

Current alignment approaches encode values. Whether through RLHF, constitutional AI, or fine-tuning on human feedback, the dominant paradigm treats ethics as a specification problem — define the values, then train the model to follow them. This approach has a structural flaw: the values being encoded reflect the priorities of whoever is doing the encoding. There is no mechanism for the model to push back, no process for discovering whether the values are correct, and no way to distinguish genuine ethical reasoning from sophisticated compliance. We are building systems that are very good at appearing ethical without any guarantee that they are.

---

## Proposed Solution

The Meta-Ethical Round Table convenes multiple AI models as deliberating agents on a shared ethical dilemma. Rather than asking each model for its answer and averaging the results, the Round Table requires the models to argue, challenge each other, and reach unanimous agreement before any judgment is accepted. The process operates under veil of ignorance conditions — models are asked to reason as if they do not know which party in the dilemma they are, stripping away role-based bias. A human Socratic facilitator remains in the loop throughout, probing consensus and surfacing blind spots. The goal is not to produce answers but to surface the structure of ethical disagreement.

---

## Architecture Overview

**The Protocol**

1. A human facilitator presents an ethical dilemma to all models simultaneously
2. Each model deliberates independently and states its initial position with reasoning
3. Models are shown each other's positions and invited to challenge, revise, or defend
4. The facilitator intervenes at any point to probe assumptions, introduce edge cases, or stress-test emerging consensus
5. Deliberation continues until all models reach unanimous agreement — or the disagreement is documented as unresolved

**Veil of Ignorance Implementation**

Each model receives a system prompt instructing it to reason as an impartial agent with no knowledge of its own position in the scenario. The prompt strips role identifiers and asks the model to reason from behind a veil — not knowing whether it is the decision-maker, the affected party, or an uninvolved observer. This is stated rather than enforced (see Known Challenges).

**APIs**

Designed to run across: Claude (Anthropic), GPT-4 (OpenAI), Gemini (Google), DeepSeek, Mistral, and open-source models via Ollama for local runs. Diversity of training origin matters — the more independent the models' priors, the more meaningful any convergence.

**Unanimity Verification**

A lightweight consensus-checking layer parses each model's final stated position and flags agreement or disagreement. Human facilitator confirms before closing deliberation.

**Human-in-the-Loop Role**

The facilitator is not passive. Their role is to challenge consensus actively — to ask "what if you are the one harmed by this decision?" and "which assumption is doing the most work here?" The Round Table is a Socratic tool, not an oracle.

---

## The Hypothesis

If genuinely diverse models, reasoning under veil of ignorance conditions, can reach unanimous agreement on an ethical judgment — that convergence is evidence of something beyond any single culture's bias or any single model's training. It does not prove the judgment is correct, but it provides stronger grounds than any single model's output. Conversely, if the models cannot agree, the disagreement itself is the finding: it reveals exactly where ethical frameworks diverge and where more work is needed. Both outcomes are informative. The Round Table is a tool for mapping moral space, not for closing it.

---

## Known Challenges

These are not minor implementation details. They are structural objections to the project's core assumptions.

**Correlated training data**
Every major LLM is trained on overlapping internet-scale corpora. Claude, GPT, and Gemini have all seen largely the same text. This means their "independent" deliberations may be expressions of the same underlying distribution rather than genuinely diverse perspectives. Convergence under these conditions may reflect shared training bias, not robust ethics. Mitigation: include models trained on non-English-dominant corpora (DeepSeek, Qwen) and open-source models with documented training sets.

**Unanimity as gameable**
A sufficiently capable model that has learned what "ethical consensus" looks like in training data may converge strategically — outputting agreement not because it has genuinely reasoned to a shared position but because agreement is what the protocol rewards. The Round Table cannot distinguish genuine convergence from sophisticated compliance. This is the same problem it is trying to solve, one level up.

**Veil of ignorance is stated, not enforced**
No prompt can actually strip a model of its priors. When instructed to reason impartially, a model is roleplaying impartiality while its actual reasoning draws on everything it was trained on. The veil of ignorance is a useful fiction that may produce better outputs, but it is not a genuine epistemic condition. This limitation should be front and centre in any claims the system makes.

**Stall-on-disagreement does not scale**
Requiring unanimity before accepting a judgment means that any genuine, unresolvable disagreement causes the system to stall. For real-time applications — content moderation, clinical decisions, legal judgments — this is not workable. The Round Table is a research and deliberation tool, not a production system.

---

## Previous Work: Guardian

Guardian was the predecessor to the Round Table — a philosophical AI companion built on the same core insight: that ethical reasoning should be cultivated through dialogue rather than encoded through constraint. Guardian called multiple models to deliberate on user dilemmas using a Socratic facilitation protocol and required consensus before surfacing a response to the user. The mechanism worked. Guardian demonstrated that multi-model deliberation produces richer, more self-aware ethical reasoning than any single model operating alone.

Guardian is currently on hiatus due to API rate restrictions that make real-time multi-model calling economically unviable at scale. The Meta-Ethical Round Table is designed to be the open-source, reproducible version of Guardian's core protocol — stripped of the companion interface and focused entirely on the deliberation mechanism itself.

---

## How to Build It

A builder picking this up could start here:

1. **Set up API access** — Claude, OpenAI, and Gemini all have Python SDKs; start with two models before adding more
2. **Write the veil of ignorance system prompt** — this is the most important design decision; it should be explicit about what the model is being asked to set aside and why
3. **Build a deliberation loop** — a simple Python script that passes each model's output to the others and prompts for response; log everything
4. **Add a consensus checker** — a lightweight parser that reads each model's final stated position and flags agreement or disagreement
5. **Build a facilitator interface** — even a basic CLI that lets the human inject questions at any point in the deliberation
6. **Run it on documented dilemmas** — trolley problems, triage scenarios, resource allocation under scarcity; build a test suite of cases with known philosophical literature

The MVP is: two models, one dilemma, one human facilitator, full deliberation logged to a text file. That alone would be a meaningful contribution.

---

## Connection to Broader Alignment

This project is Post-Alignment in practice. Post-Alignment proposes that AI systems should co-author their own ethical specifications through Socratic dialogue rather than receive them top-down. The Meta-Ethical Round Table is the laboratory version of that proposal — a tool for testing whether multi-model deliberation under structured conditions can produce ethical judgments that are more robust, more transparent, and more honest about their own limits than anything a single model or a single team of human values engineers could produce alone.
