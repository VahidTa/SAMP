# A framework for using AI Agents for projects

Many companies and engineers currently misuse AI by assuming that spinning up an agent, issuing a string of sequential prompts, and receiving a block of code equates to project completion. While more pragmatic engineers utilize localized "skills" or optimization tools to minimize token consumption, these strategies are strictly limited to small-scale projects. They break down entirely at scale because Large Language Models (LLMs) cannot effectively maintain coherent state over massive context windows over time, resulting in severe context degradation and amnesia.
Problem statements

Agents are highly efficient for small, hyper-focused tasks. However, as context grows, they begin to generate lower-quality code, introduce vulnerabilities, duplicate logic, and completely forget other parts of the codebase.

When multiple engineers collaborate on a single project, a lack of standardized boundaries leads to fragmented architectural approaches. Because engineers often fail to strictly constrain an agent's decision-making boundary, the AI is allowed to introduce conflicting or redundant logic.

While modern AI tooling leverages vector memory, commit tracking, or compression techniques to combat amnesia, we as engineers must establish an architectural framework to use these features properly.

## Framework

This design introduces a strict operational framework scalable across any discipline Software, NetDevOps, Platform Engineering, Finance, or Healthcare.
It relies on four core pillars: the Mastermind Agent, SKILL Files, Sub-Agents, and Session Memory.

### Mastermind Agent

We need a Mastermind agent to be responsible for interacting with the end user and requests. Mastermind will get the requests and based on the project “high level” knowledge, which comes from reading an standard written AGENTS.md file in the root directory, it will do the job. The Mastermind agent is only aware of the high level of the project and what the project is. Also, it is responsible to create sub-agents based on the SKILL.md files that we will discuss later. Mastermind must be restricted to know details of any module, code, config, or anything related to the projects. Only high level where those scripts exist.
Another important role of Mastermind is it must be the contact point between sub-agents. No sub-agents must communicate with each other. Instead they must ask from Mastermind, and then Mastermind communicates with the most related sub-agent or end user to get more data. For example, it will not keep the detailed data that sub-agent A is giving to B. Mastermind is there to control the requests and route the requests to related sub-agent. The sub-agent is responsible for keeping the details while running the task until it finishes. 

When the history of a running task is growing Mastermind should use MEMORY.md which I describe later. This will help the Mastermind agent to know what it is supposed to do. Mastermind agent will store “Key” in the MEMORY which refers to object storage like S3. With this approach, Mastermind keeps a lightweight pointer and reloads detail on demand. The “Key” format will be like “module:task-id:artifact-type” and brief description. For example:

- Fix issue-id 2. User login faces delay:
- users:task-788:findings
- users:task-788:request

The module identifies the domain a sub-agent owns, task-id is which request, and artifact-type will distinguish the requests, thoughts, findings etc. This is important that in AGENTS.md file it must be mentioned Mastermind is responsible for this file and if any sub-agent needs data from another sub-agent, Mastermind must check the MEMORY and check if any related task exists and then give the key back to sub-agent and sub-agent can get all related details from the S3 bucket.

### SKILL files

SKILL.md file contains the definition of tasks that sub-agent must follow. It detailed the module/script/path in the file in a way that when a sub-agent reads it, it should understand what that module is for and what it should follow. The structure already has a standard and you can find all details in https://agentskills.io/skill-creation. In summary it must follow the below:

“””
—
name: <name of the skill/sub-agent>

description: <what this module/script/path is responsible for in a high level
—

< full details of what sub-agent must know and follow to properly implement the request>

“””

This file is important. Sub-agents must follow these to get successful in the task. It is better to update this either manually or via sub-agents. Instruction must be direct and clear and not give the choice to the sub-agent which leads to different outputs. It must restrict the sub-agent to this specific module and not allow this sub-agent to go anywhere else or update any other module. 

### Sub-Agents

Sub-agents are short-lived, ephemeral entities instantiated on demand only when a specific task is created. Each sub-agent's operational boundaries and capabilities are strictly defined by a corresponding SKILL file. When a task arises, Mastermind identifies the required skill based on the task scope and provisions the appropriate sub-agent. For small-scale projects, mapping a single SKILL file to an entire directory or path may suffice. For example, consider the following structure for a basic project:

```
├── app.py
└── src
    ├── helper.py
    ├── models.py
    └── utilities.py
```

Conversely, for large-scale codebases (such as Django-based applications), it is optimal to provision one sub-agent per application module. This granular scoping ensures higher output precision, minimizes context window saturation, and effectively eliminates amnesia.

Direct inter-agent communication is strictly prohibited. All coordination must be handled via Mastermind. Sub-agents derive their domain-specific boundaries and implementation details entirely from their designated SKILL.md file. Upon task completion, the sub-agent returns the execution results to Mastermind. Sub-agents are also responsible for updating their local SKILL.md file whenever new logic is implemented or existing code structures change. However, a strict Git-based workflow must serve as a guardrail to prevent autonomous agents from committing malformed or improper changes. This ensures a clean, auditable version history, allowing future sub-agents to seamlessly resume workflows or implement subsequent modifications.


### MEMORY

In large-scale projects, Mastermind requires significant context memory to keep track of state within complex tasks. To handle this, the Mastermind agent writes down its initialisation rationale, expected outcomes, and current execution state in a local file named MEMORY.md. This file remains local and should be excluded from the Git repository. Because Mastermind owns this file, it is responsible for keeping the data structured and clean. If a user wants to purge stale data, they can simply issue a command like “clean up MEMORY.md”; Mastermind will then evaluate AGENTS.md to determine the correct cleanup policy. Architecturally, I recommend applying an S3 lifecycle policy to the object bucket; Mastermind can then periodically audit S3 and, if the underlying data has expired, delete the corresponding keys from MEMORY.md.
