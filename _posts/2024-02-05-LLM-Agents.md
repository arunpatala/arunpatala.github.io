---
layout: post
title:  "Large Language Models based Autonomous Agents"
date:   2024-02-04 11:53:18 +0530
categories: AI
---

## Table of Contents

1. [Autonomous Agent in LLMs](#autonomous-agent-in-llms)
2. [Planning Through Reasoning](#planning-through-reasoning)
3. [Simple Reasoning and Actions](#simple-reasoning-and-actions)
4. [Working with Tools](#working-with-tools)
5. [Conclusion](#conclusion)
6. [References](#references)

Autonomous Agent in LLMs
--------------------------------------------------------

## Introduction
In the realm of artificial intelligence, the concept of autonomous agents represents a significant leap towards creating systems that not only understand the world around them but also act independently to achieve specific goals. These agents are crafted to solve complex problems by reasoning, interacting with their environment, learning from experiences, and constantly adapting to new challenges. This blog post explores the role of Large Language Models (LLMs) as the cognitive core of autonomous agents, transforming them from mere question-answering tools into entities capable of executing tasks with a level of autonomy previously unattainable.

## What Are Agents?
At their core, agents are entities designed to navigate and manipulate their environment to solve problems. They are characterized by their ability to:
- **Reason about complex issues**, breaking them down into manageable steps.
- **Interact dynamically with their environment**, utilizing available tools and resources.
- **Learn from experiences**, refining their approach by building a nuanced model of their surroundings.
- **Adapt and evolve** in response to the changing state of the environment or the emergence of errors.

This multifaceted capability allows agents to undertake tasks with a degree of independence, making decisions and adjusting strategies based on their understanding and learning.

## LLMs as Autonomous Agents
A Large Language Model (LLM) is built as a text completion algorithm, primarily to help in answering questions, not necessarily to accomplish tasks. However, its strength lies in its general reasoning capabilities, showing common sense and appearing knowledgeable. This post explores how we can utilize LLMs as a core method (akin to a brain) to solve tasks like an autonomous agent, acting independently.

This area of study is rapidly evolving, characterized by multiple subproblems and a variety of approaches. Given the breadth of the field, this post will not cover everything. Instead, we'll sample a few papers to illustrate how they contribute to our understanding of agents. This should provide a taste of the different approaches being explored.


Planning Through Reasoning
--------------------------------------------------------

### Breaking Down Tasks
The essence of planning in the context of using Large Language Models (LLMs) revolves around the concept of reasoning. This involves breaking a large task into manageable sub-tasks. Rather than seeking direct answers, prompting the LLM to approach a problem step-by-step can significantly enhance accuracy. This process, often referred to as prompt engineering, may seem straightforward, yet it effectively simplifies reasoning for the LLM by guiding it through a structured thought process.

### Chain of Thought (COT) Example
Consider the "Chain of Thought" (COT) methodology as a practical example. This technique encourages the LLM to articulate its reasoning step-by-step, mirroring how a human might logically work through a problem. 

**Fig: Example COT**

### Extending to Tree of Thought
Building on this, we can develop a "Tree of Thought" where each step is further dissected into additional steps. The key challenges here include determining the appropriate depth for breaking down steps and establishing a mechanism for backtracking if a certain line of reasoning proves unfruitful.

### Application in Research
Recent research papers explore expanding this approach into a reinforcement learning setting or employing a dedicated planner. In these models, the LLM acts as a general reasoner capable of deconstructing a problem into a series of steps or even multiple solution pathways. Terms like "Chain of Thought" and "Tree of Thought" are pivotal in this discussion, highlighting the strategic use of LLMs to navigate complex problem-solving scenarios.


Simple Reasoning and Actions
--------------------------------------------------------

### The Challenge of Finite Memory
One of the inherent limitations of Large Language Models (LLMs) is their finite memory, which restricts their ability to seek out new facts or assimilate new information readily. This constraint becomes particularly evident in unfamiliar domains or settings not covered during their training phase.

### Integrating External Information Sources
Consider a scenario where an LLM needs to access up-to-date information from a Wikipedia database. The challenge is how to enable the LLM to query this database for new information without undergoing retraining. This process involves breaking down the task into a series of steps, akin to a "Chain of Thought," where each thought prompts an action (such as queries to Wikipedia) and results in an observation (the outcome of the query).

### Action-Observation as Text Completion
The beauty of this approach is that it can be framed as a text completion task for the LLM. The model outputs a query in a format that resembles a Wikipedia search, achieved through in-context learning or prompt engineering. This method provides examples of how to structure thoughts, actions, and observations to tackle the problem, potentially involving multiple action-observation cycles for each thought.

### Retrieving External Information
The ReACT framework (Retrieve and Act) exemplifies this strategy by dividing the problem-solving process into thoughts, actions, and observations. By embedding an example within the prompt, the LLM can generate a thought followed by an action (e.g., a Wikipedia search). The results, once incorporated as an observation, inform subsequent actions by the LLM.

### Extensions and Reflection
Further development can incorporate reinforcement learning to allow the LLM to learn from its actions based on feedback. This feedback, typically in the form of binary rewards indicating problem-solving success, enables the model to refine its strategies and pursue more effective problem-solving paths.



Working with Tools
--------------------------------------------------------

### Introduction to TALM and Toolformer
Recent advancements, as illustrated by papers on TALM and Toolformer, demonstrate the potential for Large Language Models (LLMs) to interact with external tools. These tools range from performing simple arithmetic calculations using calculators, fetching simple facts or answers from Wikipedia, retrieving information through Google searches, to executing external code or APIs for broader applications.

### Extending LLM Capabilities
Building on the principles similar to the ReACT framework discussed earlier, LLMs can be extended to invoke a variety of tools, obtaining relevant results directly into their processing stream. This requires a specific text format for calling tools with the necessary parameters and receiving the results as text.

#### Example Format for Tool Interaction
```
[API(input) -> (output)]

[Weather(temp, NY) -> 20C]
```
With such a format, when the LLM predicts `[API(input) ->`, the process can pause to call the actual API, subsequently integrating the output back into the text. This mechanism significantly expands the LLM's access to external tools and information, enhancing its problem-solving capabilities.

### Prompting LLM for API Calls
The challenge lies in guiding the LLM to generate such API calls accurately. It involves understanding the available APIs, determining the appropriate contexts for their use, and formatting these calls correctly. By including examples of text with API calls in the prompts, LLMs can learn to make new API calls without the need for model retuning.

### Fine-Tuning and Optimization
Further refinement is possible by evaluating whether using API calls enhances prediction accuracy. This could involve reinforcement learning or setting a threshold probability for making API calls. Such strategies can optimize the balance between leveraging external tools and relying on the LLM's inherent capabilities.



Conclusion
--------------------------------------------------------

Exploring the potential of Large Language Models (LLMs) as autonomous agents, through methodologies like Chain of Thought, ReACT, and the integration with tools such as TALM and Toolformer, showcases a promising evolution in artificial intelligence. By enhancing LLMs with the ability to reason, perform action-observation cycles, and interact with external tools without extensive retraining, we are on the brink of transforming them from mere language processors to sophisticated problem solvers. This shift not only augments their utility across various domains but also marks a significant step towards realizing AI systems that can autonomously understand, reason, and act in complex environments. As we continue to refine these models and their interactions with the digital world, the future of AI looks poised to exceed the boundaries of current capabilities, promising a new era of intelligent systems that can seamlessly integrate with and navigate the vast expanse of human knowledge and digital resources.


References
--------------------------------------------------------

- Weng, Lilian. "[Agent Models: Planning, Learning, and Language](https://lilianweng.github.io/posts/2023-06-23-agent/#component-one-planning)." Accessed at Lilian Weng's blog.
- Weng, Lilian. "[Prompt Engineering for Language Models](https://lilianweng.github.io/posts/2023-03-15-prompt-engineering/#external-apis)." Accessed at Lilian Weng's blog.
- "[Chain of Thought Prompting Elicits Reasoning in Large Language Models](https://arxiv.org/pdf/2201.11903.pdf)." Accessed at arXiv.
- "[ReACT: Retrieve-and-Edit Approach to Controllable Text Generation](https://arxiv.org/pdf/2210.03629.pdf)." Accessed at arXiv.
- "[ToolFormer: Language Models with External Tool Use](https://arxiv.org/pdf/2302.04761.pdf)." Accessed at arXiv.
- "[TALM: Tool Augmented Language Models](https://arxiv.org/pdf/2205.12255.pdf)." Accessed at arXiv.
