# WWW '26 Prompt Iteration
Here is the result of prompt iteration for each RQ in WWW '26 paper "Enhancing Content Moderation with LLMs: A Reddit Case Study on Evaluating and Refining Human Decisions".

**RQ1 Version 1** uses a prompt structure adapted from prior work on content moderation, with task-specific modifications such as explicit JSON output formatting and evaluation instructions. The system message presents subreddit rules as isolated components, requiring the model to evaluate each post against only one rule per interaction. This version achieves an accuracy of 39.7%. We observe that evaluating rules in isolation hinders the model’s ability to develop a coherent understanding of community guidelines, resulting in high rates of both false positives (incorrectly flagging compliant content) and false negatives (overlooking actual violations). This suggests that a fragmented rule presentation undermines moderation effectiveness [Click here to view system message and prompt](RQ1_version1.pdf).

**RQ1 Version 2** improves upon Version 1 by incorporating all subreddit rules into a single prompt, enabling the model to consider the full context of community guidelines during each decision. This integrated approach yields an accuracy of 63.8%, demonstrating that holistic rule presentation supports better judgment [Click here to view system message and prompt](RQ1_version2.pdf).

**RQ1 Version 3** retains the integrated rule set from Version 2 and introduces additional toxicity metrics from Perspective API, including six different metrics (Toxicity, Severe Toxicity, Identity Attack, Insult, Profanity and Threat). The goal is to provide supplementary signals for violation detection. However, accuracy performance slightly decreases to 61.1%. The results indicate that generic toxicity metrics introduce noise and are misaligned with context-specific rule violations, leading to misclassifications of posts that were correctly judged in Version 2. Consequently, toxicity metrics are omitted in later versions [Click here to view system message and prompt](RQ1_version3.pdf).

**RQ1 Version 4** introduces few-shot examples to improve contextual understanding. The prompt includes four illustrative examples (two violation posts and two compliant posts) from the relevant subreddit, each accompanied by human-provided rationales and specific rule references. This offers concrete demonstrations of rule application and raises accuracy to 75.4% [Click here to view system message and prompt](RQ1_version4.pdf).

**RQ1 Version 5** incorporates a chain-of-thought (CoT) reasoning structure that breaks the moderation process into five discrete steps: 
1. Understand the post's substance.
2. Review the post against each subreddit rule.
3. Identify any rules that have been violated.
4. Determine whether a post violates the rules and determine its confidence level.
5. Compile conclusions into the required JSON structure.
This structured reasoning process ensures thorough and transparent evaluation, further improving accuracy to 79.2% [Click here to view system message and prompt](RQ1_version5.pdf).
