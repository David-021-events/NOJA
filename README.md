# NOJA
A structural specification for accountable human judgment in AI-enabled systems

### **What NOJA is**

NOJA (Nodes of Judgment Architecture) is a way to make human accountability visible in AI-enabled systems. AI can predict, recommend, route, generate, and act, but it cannot be accountable. Accountability belongs to the named roles that authorize the policies, thresholds, prompts, tools, and fallback rules that govern what the system does. It's deployed bottom-up, one trained person at a time, not through top-down audits or org-wide rollouts.

NOJA gives organizations a way to answer three practical questions about any automated outcome:

* Who authorized this?  
* What exactly did they authorize?  
* Can that authorization be revised if the system gets something wrong?

**The core purpose is to prevent organizations from hiding human judgment inside machines without a clear owner, signature, scope, or fallback.**

### **The problem**

AI agents are often built as one big black box: the same system observes, interprets, decides, and acts inside a single loop. When something goes wrong, you often cannot tell whether the model made a bad prediction, whether the policy was wrong, whether the prompt contained hidden judgment, whether the tool acted incorrectly, whether the human reviewer misunderstood their role, or whether the decision logic was ever authorized at all.

This isn't only an AI problem. Many older systems already have it: years of code, spreadsheets, approvals, and informal routines blur until no one can say where prediction ends and judgment begins. What's different about agentic AI is that this collapse is now treated as a design goal rather than a defect. *NOJA rejects that framing:* if a system is shaping important decisions, the human judgment inside it must be visible, signed, scoped, and revisable.

### **The basic unit: a judgment node**

Every important decision lives inside a *judgment node* with four distinguishable parts:

* **Prediction** — what the system thinks is true or likely ("this customer may default," "this transaction looks suspicious"). Predictions are inputs; they don't decide what should happen.  
* **Judgment** — the policy that interprets the prediction ("if default risk is above this threshold, escalate"). This is where risk tolerance, business rules, and context live, and where human accountability enters the system.  
* **Execution** — what actually happens: a rule fires, an algorithm runs, an agent acts, or a human does the work.  
* **Accountability** — exactly one named role owns the judgment in this node. Not "the AI," not "the team." One role. Machines can simulate agency but cannot bear accountability.

### **The key mechanism: signatures**

The most important concept in NOJA is the *signature*: a named role's authorization of a specific piece of judgment. A signature is not a general approval of "the AI system" — it attaches to a specific artifact for a specific use under specific conditions. For complex AI, this often means signing a composite bundle: the policy, system prompt, pinned model version, tool definitions, and tests showing the system behaves within an acceptable envelope.

Two modes of authorization are recognized. 

1. *Design-time judgment* signs the policy in advance, and the system applies it across many cases — for example, the Head of Credit Risk signs a policy specifying which loans auto-approve, auto-deny, or escalate. 

2. *Human-in-the-loop* routes a live case to a named human who decides that case. Both can be legitimate, but HITL is not automatically safer. A well-designed policy signed in a quiet room is often safer than a human catching mistakes in a live stream.

Every signature also needs a *scope* and a *fallback*. Scope makes clear when the authorization applies: this product line, this default-rate band, this model version, this volume cap, this duration. 

Policies don't decay because time passes; they decay because the conditions they were signed under no longer hold, and signatures lapse automatically when scope breaks. Fallbacks specify what happens when a signature is pulled or lapses: halt, safe mode, drain in-flight work then halt, or cut over to a pre-signed successor.

### **Networks and the granularity rule**

Real systems are chains of nodes: one node's output feeds another's input. Two structural requirements apply: 

1. every node has one accountable role, and   
2. every connection is traceable. 

From any outcome, the path back through prediction, signed artifact, execution, accountable role, and upstream decisions must be reconstructible.

The most useful practical test is the *granularity rule*:

A judgment node is correctly sized when a rejection of its output can be traced to a single signature that can be revised to prevent the same error from recurring.

If you can't point to the specific signed policy that needs to change, the node is too big. This rule prevents the common failure pattern of a giant end-to-end agent with a human approver at the boundary. 

The approver is accountable for the boundary call, not for the hidden policies, prompts, tool choices, or subagent decisions inside the agent. If the internal process contains policy-shaped logic that no one separately authorized, the node fails the rule and must be decomposed.

### **How to put it into practice**

NOJA is easier to deploy than most governance frameworks because you don't roll it out. It spreads. The path that works in practice is bottom-up and viral, and each step earns the next.

**Start with one person** in one department whose accountability is already clear. They're the named person responsible for what they produce. That existing accountability is the foundation; you're not creating it, you're keeping it visible as their work shifts to AI tools. Teach them agentic AI alongside NOJA's core principles: the four parts of a judgment node, what a signature does, why scope and fallback matter. They begin using approved AI within their normal work.

Two things happen, both visible and measurable before any scale-up commitment. 

1. The person produces clear gains in quality and throughput.  
2. Their work surfaces bottlenecks within the department. They are a forcing function for change. 

The department head sees this and requests broader training. The demand is pull, not push: it comes from someone who's seen the value firsthand. At department level, training and system design happen together. 

NOJA's three levels are used as lenses during the work, not as a sequential rollout. As a process is redesigned, the team asks:

**Level 1** (Boundary) → who owns this decision?  
**Level 2** (Signature) → is the policy actually signed?  
**Level 3** (Lifecycle) → are the conditions of that signature still valid?

Conformance isn't bolted on afterward; it actually shapes the design as it happens, which is what makes the result reliable.

Once a department operates this way, it becomes a forcing function for the rest of the organization. Its work surfaces bottlenecks in adjacent departments; those departments get trained next; the cascade continues. Each new department has visible proof from the prior one before being asked to change.

This is why the pattern works in months not years, and in reality why it works at all. It's affordable because nobody is asked to inventory and sign everything upfront. It's reliable because each step is validated by visible results before the next is attempted. The architecture grows alongside the AI adoption that's happening anyway, rather than arriving as a separate, expensive overlay.

### **What you'll find when you start**

Most organizations are already in a state of pre-agentic collapse. Important decisions are spread across legacy software, spreadsheets, informal approvals, and institutional habits. 

AI didn't create that condition; it exposes it. As the trained person's work, and then the department's, starts running through NOJA's lenses, the gaps surface concretely: no one owns a key threshold, a prompt contains hidden policy, a model is being used outside its approved context, or a legacy process has been making judgment calls for years without a clear owner.

That is the value of NOJA. It does not attempt to make systems accountable.  People are accountable, and NOJA makes that accountability visible, traceable, and revisable.

