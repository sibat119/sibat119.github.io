# Three Motivating Examples


### Example 1: Bias and Social Reasoning in a Simulated Courtroom

Traditional bias benchmarks often present a predefined situation and ask a model to answer a question or make a judgment. Such evaluations can identify important biases, but they provide limited evidence about how bias influences decisions across complex social interactions.

Consider instead a simulated courtroom or society populated by autonomous characters. Each character has configurable beliefs, goals, relationships, knowledge, and behavioral tendencies. The environment contains an underlying set of facts, but characters may have incomplete or incorrect beliefs about those facts. They interact, exchange information, and make decisions that change how the situation unfolds.

An AI agent operating in this environment must interpret evidence, reason about other characters' beliefs, make decisions under uncertainty, and update its understanding as new information emerges. By changing the beliefs, demographic attributes, information access, and behavior of different characters, we can construct counterfactual situations and examine what actually influences the agent's decisions.

The goal is not simply to determine whether an isolated response contains bias. It is to investigate how bias emerges, persists, or changes through interactions, and whether the agent can distinguish factual evidence from unreliable beliefs and social influences. Such environments could also support multimodal evaluation through speech, visual observations, and other forms of interaction.

### Example 2: Financial Reasoning in a Simulated Economy

Financial prediction benchmarks often rely on historical market data. While useful, they can reward models for recognizing familiar companies, events, and market patterns without establishing how well those models adapt to unfamiliar economic conditions.

Consider a simulated financial world containing autonomous companies, investors, financial institutions, markets, and news generators. Each entity operates according to configurable characteristics and behavioral rules. The economic environment evolves through interactions among these entities, changes in economic conditions, and the consequences of earlier decisions.

In one simulated world, a historically dominant technology company may be a small, struggling firm. An unfamiliar company may become the market leader. Investors may behave differently from those in historical markets, and familiar economic relationships may change. Models must make predictions and decisions based on evidence from the simulated world rather than relying exclusively on learned associations from the real world.

By systematically varying company fundamentals, investor behavior, information quality, and economic conditions, we can investigate whether models understand the relationships relevant to financial decisions and adapt when those relationships change.

The goal is not merely to create additional financial scenarios. It is to evaluate forecasting, uncertainty estimation, belief updating, and decision-making across different economic environments. The central challenge is determining whether simulated market behavior captures the mechanisms necessary for meaningful evaluation.

### Example 3: Adaptive Automation in a Simulated Software Infrastructure

Existing software automation benchmarks often begin with a predefined task: configure a server, deploy a service, repair a defect, or reach a desired system state. Successfully completing such tasks provides evidence of automation capability, but it does not establish whether an agent can maintain a reliable system as requirements and operating conditions evolve.

Consider a simulated computing infrastructure containing servers, services, applications, databases, users, and security threats. Users interact with the system continuously, producing changing workloads and requirements. Software components fail, resource demands change, and automation decisions can introduce new performance or security problems.

An AI agent must observe system behavior, infer emerging user and operational needs, decide when intervention is necessary, and perform appropriate automation. It must also recognize when no action is needed and evaluate the consequences of its previous interventions.

Different configurations of users, applications, infrastructure, and failure conditions produce different operating environments. By varying these conditions, we can measure whether the agent maintains availability, performance, security, and resource efficiency across unfamiliar situations.

Unlike isolated automation tasks, this evaluation measures sustained operational competence. It tests whether an agent can understand and manage an evolving system rather than merely execute a predefined instruction.

### Common Research Vision

All three examples examine the same fundamental limitation: successful performance on an isolated task may not provide sufficient evidence that an AI system possesses the broader capability required in a real-world setting.

The proposed direction is to evaluate AI systems through their behavior in controlled, evolving environments. These environments would contain interacting entities, changing conditions, incomplete information, and observable consequences. Their parameters would be systematically varied to examine how model behavior changes across unfamiliar situations.

The long-term goal is to establish a methodology for constructing such environments, measuring capabilities through interactions and decisions, and determining when the resulting evidence generalizes beyond the simulation.