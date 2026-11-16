---
title: "Evaluation with Evolving Worlds"
alternative_title: "Evolving Worlds as Evaluation Settings"
status: research-draft
updated: 2026-11-15
---

# Evaluation with Evolving Worlds

> Working draft. This file is currently a research map and article outline, not a publication-ready post.

## Central question

Most AI evaluations begin with a fixed task and end with a response or terminal outcome. But many capabilities we care about only become visible across interaction: whether an agent updates its beliefs, distinguishes reliable evidence from social influence, adapts when familiar relationships change, anticipates consequences, recovers from mistakes, and knows when not to act.

This article asks whether a **controlled, evolving world** can serve as a better evaluation setting. Such a world contains actors, states, rules, incomplete information, and consequences. The evaluator can systematically vary those components, observe behavior over time, and test whether success persists in unfamiliar configurations.

The intended argument is not simply that simulations are useful. It is that controlled worlds may let us make stronger capability claims than isolated tasks do—provided that the world preserves the mechanisms relevant to the claim and that success in the world transfers beyond it.

## A terminology problem to resolve first

The literature uses *world model* for several related but different objects. The blog should distinguish at least three roles:

1. **The world model as the object being evaluated.** Does a learned model correctly represent dynamics, causality, state, or possible futures?
2. **The world model as an evaluation environment.** Can a learned simulator replace or approximate costly real-world interaction while preserving policy rankings or failure modes?
3. **The world as the evaluation setting.** Can a rule-based, learned, agent-based, or hybrid environment expose sustained capabilities through interaction and change?

The third role is the main subject of this article. The first two supply architectures, validation methods, and warnings that the third role needs.

## Three world-model traditions

These are best presented as three traditions rather than three mutually exclusive model classes. Modern systems frequently combine them.

### 1. Generative, observation-space simulators

This tradition predicts what the world will look like after an action, often as pixels or video. Its advantage is that it exposes a rich observation interface that an existing vision or vision-language policy can consume. Its central risk is that visual plausibility can conceal incorrect dynamics.

The clearest evaluation-centered example is **WorldGym**, by Julian Quevedo, Ansh Kumar Sharma, Yixiang Sun, Varad Suryavanshi, Percy Liang, and Sherry Yang. WorldGym uses an autoregressive, action-conditioned video model as a proxy environment for robot-policy evaluation. It asks whether simulated rollouts can estimate real-world success and preserve relative policy rankings.

Key source:

- [WorldGym: World Model as An Environment for Policy Evaluation](https://proceedings.iclr.cc/paper_files/paper/2026/hash/7f5e909ac0324db03506b380c695ffaf-Abstract-Conference.html) — ICLR 2026.

Questions for the blog:

- Is visual fidelity necessary, sufficient, or sometimes irrelevant to evaluation validity?
- Which errors preserve policy rankings, and which reverse them?
- How should uncertainty from the simulator and uncertainty from the reward model be separated?
- Can the simulator generate counterfactual worlds outside the real-world data distribution without becoming ungrounded?

### 2. Latent predictive models and JEPA

Joint-Embedding Predictive Architectures predict in representation space rather than reconstructing every pixel or token. The motivation is to preserve task-relevant structure while discarding unpredictable or irrelevant detail. In Yann LeCun's broader proposal, a hierarchical JEPA supports prediction at multiple levels of abstraction and time scales.

Key sources:

- [Yann LeCun's proposed world-model architecture](https://ai.meta.com/blog/yann-lecun-advances-in-ai-research/) — the world model predicts missing and future states, including action-conditioned futures, in abstract representation space.
- [Self-Supervised Learning from Images with a Joint-Embedding Predictive Architecture](https://ai.meta.com/research/publications/self-supervised-learning-from-images-with-a-joint-embedding-predictive-architecture/) — I-JEPA.
- [Introducing the V-JEPA 2 world model and new benchmarks for physical reasoning](https://ai.meta.com/blog/v-jepa-2-world-model-benchmarks/) — video understanding, prediction, and action-conditioned planning.
- [TD-JEPA: Latent-predictive Representations for Zero-Shot Reinforcement Learning](https://proceedings.iclr.cc/paper_files/paper/2026/hash/3d158f054ff0cb83397367234899db07-Abstract-Conference.html) — ICLR 2026.

Questions for the blog:

- Which facts must remain decodable from a latent state for it to support an evaluation?
- If a latent world omits visually irrelevant details, could it also omit socially or ethically relevant details?
- How do we audit a world whose internal state is not directly human-interpretable?
- Can latent-space distance serve as evidence of equivalent behavior, or only equivalent representation?

### 3. Dynamics models from model-based reinforcement learning

The model-based RL tradition learns a transition model—often together with reward, value, or termination models—and uses imagined rollouts for planning, policy learning, or off-policy evaluation. The world is usually formalized through states, actions, transition dynamics, observations, and rewards.

Foundational and current sources:

- [World Models](https://arxiv.org/abs/1803.10122) — Ha and Schmidhuber's compressed spatial and temporal model, including policy learning inside imagined rollouts.
- [ADM-v2: Pursuing Full-Horizon Roll-out in Dynamics Models for Offline Policy Learning and Evaluation](https://proceedings.iclr.cc/paper_files/paper/2026/hash/1651a9bfa452a13c9f26f71bfef1f68c-Abstract-Conference.html) — ICLR 2026.
- [Reinforcement World Model Learning for LLM-based Agents](https://arxiv.org/abs/2602.05842) — NeurIPS 2026; learns action-conditioned textual dynamics using a sim-to-real-gap reward.

Questions for the blog:

- When does compounding model error invalidate long-horizon evaluation?
- Does optimizing a model for reward prediction hide failures in other aspects of the world?
- How should an evolving-world evaluation score exploration, calibration, recovery, and appropriate inaction in addition to reward?
- Can held-out dynamics and counterfactual transition rules reveal whether a policy learned mechanisms rather than a task-specific strategy?

## Proposed synthesis

The three traditions make different tradeoffs:

| Tradition | Primary prediction space | Main strength for evaluation | Main validity risk |
|---|---|---|---|
| Generative simulator | Pixels, video, text, or other observations | Rich interface; existing agents can interact directly | Plausible observations with incorrect dynamics |
| JEPA / latent predictive | Learned abstract representations | Efficient prediction of task-relevant structure | Important details may be discarded or inaccessible |
| Model-based RL | States, transitions, rewards, values | Explicit action–consequence loop and policy evaluation | Reward misspecification and compounding rollout error |

An evolving evaluation world may be hybrid: symbolic rules for invariants, a learned model for open-ended observations, agent-based actors for social dynamics, and an execution engine for verifiable consequences.

## Conference-paper sweep: inclusion criteria

The paper review uses two relevance levels.

**Core:** The paper uses a learned or simulated world as an evaluation environment, constructs a dynamic/interactive world specifically for evaluation, or directly studies whether a world model is valid for evaluating behavior.

**Adjacent:** The paper trains or evaluates a world model, uses world-model rollouts for planning, or develops simulation infrastructure that informs the design and validation of evolving-world evaluations.

Static benchmarks that merely use the word *world*, and world-model papers concerned only with visual generation quality, are excluded unless they contribute a relevant validity result.

## ACL 2026

### Core papers

1. [Multimodal Safety Evaluation in Generative Agent Social Simulations](https://aclanthology.org/2026.acl-long.1915/) — Gonzalez et al. A dynamic social simulation evaluates whether multimodal agents detect unsafe situations, revise plans, and propagate or correct unsafe behavior over extended interaction. This is the closest match to the courtroom/society motivation.
2. [The Agent's First Day: Benchmarking Learning, Exploration, and Scheduling in the Workplace Scenarios](https://aclanthology.org/2026.findings-acl.1505/) — Fu et al. TraineeBench is explicitly framed as a dynamic evaluation environment with streaming tasks, uncertainty, active exploration, and continual learning.
3. [SGVEF-LOOP: Coverage-Guided Progressive Topological Exploration and Fact-Grounded Metamorphic Evaluation for MCP Agents](https://aclanthology.org/2026.acl-long.1224/) — Liu and Zhang. Uses a closed loop of environment exploration, metamorphic case generation, and execution feedback to evaluate agents in a dynamic tool space.
4. [AgencyBench: Benchmarking the Frontiers of Autonomous Agents in 1M-Token Real-World Contexts](https://aclanthology.org/2026.acl-long.337/) — Li et al. Combines long-horizon tasks, a simulated user, and sandboxed execution. Relevant as an example of evaluation moving from fixed answers toward trajectories and deliverables.
5. [Efficient Agent Evaluation via Diversity-Guided User Simulation](https://aclanthology.org/2026.acl-industry.112/) — Nakash et al. DIVERT branches from saved interaction states to explore rare user behaviors and deeper failure modes.

### Adjacent world-model papers

6. [From Word to World: Can Large Language Models be Implicit Text-based World Models?](https://aclanthology.org/2026.acl-long.366/) — Li et al. Proposes a three-level evaluation of text world models: fidelity and consistency, scalability and robustness, and agent utility.
7. [Current Agents Fail to Leverage World Model as Tool for Foresight](https://aclanthology.org/2026.acl-long.623/) — Qian et al. Evaluates whether agents strategically invoke and use external world-model simulations before acting.
8. [Model-Based Imaginative Planning for Embodied Agents](https://aclanthology.org/2026.acl-long.827/) — Song et al. Couples an object-centric world model with an LLM for counterfactual rollouts and online refinement in partially observed environments.

## EMNLP 2026

### Core papers

1. [LogicEnvGen: Task-Logic Driven Generation of Diverse Simulated Environments for Embodied AI](https://aclanthology.org/2026.emnlp-main.74/) — Wang et al. Treats simulated environments as test cases, generates logically distinct trajectories and worlds, and measures their ability to reveal agent faults. This is a central paper for the blog.
2. [KnowSim: Evaluating Information Calibration in LLM Assistants with User Simulators that Learn](https://aclanthology.org/2026.emnlp-main.1307/) — Lee et al. Relevant because the simulated user changes through interaction rather than remaining a fixed persona.
3. [Beyond Offline A/B Testing: Context-Aware Agent Simulation for Recommender System Evaluation](https://aclanthology.org/2026.emnlp-industry.2/) — Bougie et al. ContextSim embeds simulated users within daily-life context and validates simulator-based evaluation against real engagement.
4. [GAUGE: When Not to Trust LLM-as-a-Judge in User-Simulated Evaluation of Task-Oriented Agents](https://aclanthology.org/2026.emnlp-industry.190/) — directly relevant to the validity of simulation-based evaluation and automated judging.
5. [FRESCO: Benchmarking and Optimizing Re-rankers for Evolving Semantic Conflict in Retrieval-Augmented Generation](https://aclanthology.org/2026.findings-emnlp.54/) — a temporally evolving information setting rather than a full world model; useful as a boundary case.

### Adjacent world-model papers

6. [Internalizing the Future: A Unified Agentic Training Paradigm for World Model Planning](https://aclanthology.org/2026.emnlp-main.698/) — Zhang et al. Trains an agent to generate prospective state rollouts and calibrated success estimates.
7. [Policy and World Modeling Co-Training for Language Agents](https://aclanthology.org/2026.emnlp-main.1885/) — Lu et al. Uses on-policy transition data as auxiliary world-model supervision.
8. [Why Do LLM Agents Fail in Exploring New Environments? A World-Modeling Perspective](https://aclanthology.org/2026.findings-emnlp.1023/) — Chen et al. Connects exploration collapse to weak state and transition grounding in unfamiliar environments.
9. [Better World Models Can Lead to Better Post-Training Performance](https://aclanthology.org/2026.findings-emnlp.1012/) — Gupta et al. Studies how explicit world-model supervision affects representations and later RL performance.
10. [How do World Models and Policies Compose in LLM Agents? A Joint Spectral and Behavioral Account](https://aclanthology.org/2026.findings-emnlp.1651/) — studies the interface between world knowledge and policy learning.
11. [MemWM: Memory-Augmented Text-Based World Model](https://aclanthology.org/2026.emnlp-main.2221/) — relevant to maintaining coherent state across long trajectories.

## ICLR 2026

### Core papers

1. [WorldGym: World Model as An Environment for Policy Evaluation](https://proceedings.iclr.cc/paper_files/paper/2026/hash/7f5e909ac0324db03506b380c695ffaf-Abstract-Conference.html) — Quevedo et al. The most direct example of a learned world model used as an evaluation environment.
2. [World-In-World: World Models in a Closed-Loop World](https://proceedings.iclr.cc/paper_files/paper/2026/hash/5b4263be85820683d78675cc18d2efc7-Abstract-Conference.html) — Zhang et al. Evaluates heterogeneous world models by their downstream closed-loop task success rather than visual quality alone.
3. [SimuHome: A Temporal- and Environment-Aware Benchmark for Smart Home LLM Agents](https://openreview.net/pdf?id=LCS1WsGvha) — Seo et al. A time-accelerated environment in which actions continuously affect temperature, humidity, and device state.
4. [Virtual Community: An Open World for Humans, Robots, and Society](https://proceedings.iclr.cc/paper_files/paper/2026/hash/68933e3533add841e115a5605c76eeba-Abstract-Conference.html) — Zhou et al. A multi-agent physics simulator and environment-generation pipeline for open-world social and robotic evaluation.
5. [GEM: A Gym for Generalist LLMs](https://proceedings.iclr.cc/paper_files/paper/2026/hash/b3956812189001f673d29c626bc897ae-Abstract-Conference.html) — Liu et al. Standardizes environment-agent interaction across 24 environments and supports both learning and evaluation.
6. [SimBench: Benchmarking the Ability of Large Language Models to Simulate Human Behaviors](https://proceedings.iclr.cc/paper_files/paper/2026/hash/2fc4d6a6400addda7493b9db258c34e0-Abstract-Conference.html) — Hu et al. Evaluates whether LLM-generated human behavior is faithful enough to support social and behavioral simulation.

### Adjacent world-model papers

7. [R-WoM: Retrieval-augmented World Model for Computer-use Agents](https://openreview.net/pdf/0bf71ba90fdb4c0075002d57b26416a103c5efc0.pdf) — evaluates next-state prediction and long-horizon procedural simulation in digital environments.
8. [Code World Models for General Game Playing](https://openreview.net/pdf?id=1UoB7IWiku) — constructs executable symbolic world models from natural-language rules and trajectories.
9. [GenEnv: Difficulty-Aligned Co-Evolution Between LLM Agents and Environment Simulators](https://openreview.net/attachment?id=5vsYRklFJf&name=pdf) — the environment evolves its task distribution with the agent's capability.
10. [TD-JEPA: Latent-predictive Representations for Zero-Shot Reinforcement Learning](https://proceedings.iclr.cc/paper_files/paper/2026/hash/3d158f054ff0cb83397367234899db07-Abstract-Conference.html) — a direct bridge between JEPA and RL world modeling.
11. [ADM-v2: Pursuing Full-Horizon Roll-out in Dynamics Models for Offline Policy Learning and Evaluation](https://proceedings.iclr.cc/paper_files/paper/2026/hash/1651a9bfa452a13c9f26f71bfef1f68c-Abstract-Conference.html) — studies when learned dynamics can support full-horizon policy evaluation.
12. [From Observations to Events: Event-Aware World Models for Reinforcement Learning](https://proceedings.iclr.cc/paper_files/paper/2026/hash/fc09b26b85ab3abb2832bd555a2e4215-Abstract-Conference.html) — abstracts continuous observations into events, relevant to deciding what mechanisms an evaluation world must preserve.
13. [Empowering Multi-Robot Cooperation via Sequential World Models](https://proceedings.iclr.cc/paper_files/paper/2026/hash/16415eed5a0a121bfce79924db05d3fe-Abstract-Conference.html) — multi-agent dynamics and predictive adaptation.
14. **Vid2World** — ICLR 2026 paper using video diffusion for action-conditioned world modeling and real-to-sim policy evaluation. The proceedings record should be added by title once its abstract page is resolved from the PDF record.

### Important exclusion

- **Benchmarking World-Model Learning / WorldTest** is highly relevant conceptually, but the ICLR 2026 submission was not accepted there; it appeared at ICML 2026. It should be discussed as related work, not labeled an ICLR 2026 paper: [PMLR record](https://proceedings.mlr.press/v306/warrier26a.html).

## NeurIPS 2026

NeurIPS 2026 had not yet taken place on this draft's cutoff date. The official accepted-title/download index was available, but final proceedings metadata was incomplete. The items below should therefore be rechecked against the final proceedings before publication.

### Core or high-priority papers

1. **ClinMAS: A Knowledge-Grounded Multi-Agent Simulation Framework for Evaluating Clinical Reasoning in LLMs** — NeurIPS 2026 Evaluations and Datasets Track. Simulates the evolving doctor–patient diagnostic process rather than evaluating isolated medical answers.
2. [Open-World Evaluations for Measuring Frontier AI Capabilities](https://arxiv.org/abs/2605.20520) — argues for long-horizon, messy, real-world evaluation as a complement to precisely specified benchmarks. This is not a world-model paper, but it helps motivate why evolving settings matter.
3. [GEBench: Benchmarking Image Generation Models as GUI Environments](https://arxiv.org/abs/2602.09007) — tests whether generative models preserve interaction logic and temporal coherence when used as GUI environments.
4. **MindGames: A Multi-Agent Benchmark and Trajectory Dataset for Evaluating Social and Strategic Reasoning in LLMs** — NeurIPS 2026 Evaluations and Datasets Track. Relevant to interactive social reasoning and trajectory-level evaluation.
5. **Evaluating Physical Reasoning in LLM Agents Requires Construction Benchmarks** — position paper arguing that physical reasoning should be tested through interactive construction under realistic physics and functional verification.
6. **AgentPulse: A Continuous Multi-Signal Framework for Evaluating AI Agents in Deployment** — relevant to evaluation under changing deployment conditions, although it is not itself a world simulator.

### Adjacent world-model papers

7. [Reinforcement World Model Learning for LLM-based Agents](https://arxiv.org/abs/2602.05842) — Yu et al. Trains action-conditioned textual world models with a semantic sim-to-real-gap reward.
8. **Kosmoi: Counterfactual World Modeling for Action-Aligned Autonomous Driving** — counterfactual dynamics for action-conditioned reasoning; final proceedings link to be added.
9. **Post-Outcome Failure Triage in World-Model RL: Auditing a Realized-Shortfall Selector Under Matched Late-Update Controls** — potentially relevant to diagnosing failures in world-model-based evaluation; requires full-paper screening.
10. **Evaluating Spatial World Modeling in Video Generators via 3D Camera Trajectory Generation** — evaluates spatial consistency in generative world models; relevant mainly to validating the simulator rather than evaluating an agent.

## What the 2026 literature appears to be saying

The papers cluster around four shifts:

1. **From static items to trajectories.** The unit of evidence is increasingly an interaction history, not a single response.
2. **From fixed environments to generated or adaptive environments.** LogicEnvGen and GenEnv explicitly vary the world or its difficulty to reveal new failures.
3. **From visual plausibility to behavioral utility.** WorldGym and World-In-World ask whether a world model preserves policy performance and task success, not merely whether its videos look realistic.
4. **From a single score to mechanism-sensitive evidence.** Dynamic worlds expose belief updating, exploration, calibration, recovery, social propagation, and long-horizon consequence management.

## The validity problem

Using a world for evaluation introduces a second object that must itself be evaluated. A score is meaningful only if:

1. the world includes the mechanisms required by the capability claim;
2. its transition dynamics are sufficiently faithful for the decisions being tested;
3. agents cannot exploit simulator artifacts unavailable in the target setting;
4. the evaluator separates policy failure from simulator failure;
5. counterfactual variations preserve the construct while changing the intended factor;
6. conclusions transfer across held-out world configurations and, where possible, to real behavior.

This suggests a two-level evaluation:

```text
Validate the world  ->  Evaluate the agent inside the world
        |                              |
mechanism fidelity              behavioral trajectory
transition validity             adaptation and consequences
sim-to-real agreement           counterfactual consistency
```

## Candidate article structure

1. **Why a correct answer is not enough**
2. **What do we mean by a world model?**
3. **Three traditions: generative simulators, JEPA, and model-based RL**
4. **Three roles: model under evaluation, evaluator, and evaluation setting**
5. **What changed in the 2026 conference literature?**
6. **Examples: society, economy, and software infrastructure**
7. **The validity problem: who evaluates the world?**
8. **A research agenda for evolving-world evaluation**

## Proposed thesis paragraph

An evolving world is useful for evaluation not because simulation is automatically more realistic than a benchmark, but because it makes different evidence possible. It lets us intervene on actors, information, rules, and environmental conditions; observe decisions and their consequences over time; and ask whether behavior survives counterfactual change. The price of that richer evidence is an additional validity burden: before trusting what the world says about an agent, we must establish what the world preserves, what it omits, and where its own learned dynamics fail.

## Reading checklist for the next pass

For every core paper, record:

- What is represented as state?
- What changes during an episode and across episodes?
- Which components are learned, scripted, retrieved, or executed?
- How are actors, goals, beliefs, and information access configured?
- What behavior is measured over time?
- What counterfactuals are possible?
- How is simulator or user-model fidelity validated?
- Is there a real-world comparison?
- Can the tested agent detect that it is in a simulation?
- What capability claim do the authors make, and does the evidence support it?

## Research status and limitations

- ACL and EMNLP entries were checked against ACL Anthology 2026 proceedings pages.
- ICLR entries were checked against the official ICLR 2026 proceedings or published OpenReview records.
- NeurIPS entries were screened from the official 2026 accepted-title/download index and primary paper pages where available; final track and proceedings links still require a post-conference audit.
- The current list includes main, Findings, industry, and evaluation/dataset tracks when directly relevant. Workshop papers are not included.
- “All papers” is operationalized through title/abstract screening for: *world model*, *simulator*, *simulation*, *dynamic environment*, *evolving environment*, *interactive evaluation*, *user simulation*, *closed loop*, *environment generation*, and *policy evaluation*. A final full-proceedings metadata sweep should be run immediately before publishing the blog.
