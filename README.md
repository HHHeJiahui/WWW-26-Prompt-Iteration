# WWW '26 Prompt Iteration
Here is the result of prompt iteration for each RQ in WWW '26 paper "Enhancing Content Moderation with LLMs: A Reddit Case Study on Evaluating and Refining Human Decisions".

## RQ1

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

## RQ2

**RQ2 Version 1** introduces a CoT mechanism that guides the LLM through a structured reasoning process to evaluate moderator decisions. The prompt provides separate reasoning flows for violation posts and compliant posts. This version achieves an accuracy of 67.0%. However, analysis reveals a critical limitation: the model treats moderator decisions on violation posts as incorrect if the cited rule is inaccurate, even when the removal itself is justified. This overly strict criterion leads to false negatives in evaluation [Click here to view system message and prompt](RQ2_version1.pdf).

**RQ2 Version 2** addresses this issue by introducing an additional output field: cite_wrong_rule, a Boolean flag indicating whether the moderator cited an incorrect rule while still correctly removing a violating post. This refinement allows the model to distinguish between removal accuracy and rule citation accuracy, increasing overall performance to 72.9%. Further error analysis reveals additional failure modes, such as ambiguous rule interpretation and undetected violations, suggesting a need for more granular error categories [Click here to view system message and prompt](RQ2_version2.pdf).

**RQ2 Version 3** introduces a finer-grained taxonomy of error causes and revises the correctness criteria for moderator decisions on violation posts. A decision is now considered correct if the post violates any subreddit rule and was removed, regardless of the specific rule cited. The prompt also categorizes errors into three types: (1) citing an incorrect rule, (2) citing an overly vague rule, and (3) missing a violation entirely. These adjustments significantly improve performance, yielding an accuracy of 80.6% [Click here to view system message and prompt](RQ2_version3.pdf).

## RQ3

**RQ3 Version 1** introduces a CoT mechanism that instructs the LLM to analyze five violating and five compliant examples related to an ambiguous rule. The model is prompted to identify why the rule is ambiguous, distinguish between violating and compliant cases, and propose a refined version with clearer criteria. Results from this version are unsatisfactory. We attribute this to the limited number of examples, which constrains the model’s ability to discern robust patterns. Moreover, the proposed rules often diverge significantly from the original and tend to overfit the provided examples, reducing generalizability [Click here to view system message and prompt](RQ3_version1.pdf).

**RQ3 Version 2** increases the number of examples to ten per category. Compliant examples are specifically chosen from highly up-voted posts within the subreddit, as these are indicative of full rule compliance. The prompt is modified to emphasize that refinements should preserve the original rule’s intent.  This version produces rules that align more closely with the original purpose. However, we note that outputs vary across subreddits, suggesting that LLMs may interpret violations and compliance differently and employ varying linguistic expressions when refining rules [Click here to view system message and prompt](RQ3_version2.pdf).

**RQ3 Version 3** modifies the task to generate multiple candidate rules instead of a single output, and then rank them by priority. This approach encourages the model to consider a broader range of possibilities. While this helps capture more nuance, the generated rules still exhibit ambiguity in certain forums. Additionally, the model frequently produces redundant and highly similar candidates, indicating limited diversity in proposed solutions [Click here to view system message and prompt](RQ3_version3.pdf).

**RQ3 Version 4** enhances the system message by incorporating principles of high-quality rule design (including clarity, objectivity, and actionability) along with common pitfalls to avoid. The prompt is also adjusted to limit output to a single candidate rule, reducing redundancy and focusing the model on producing one well-justified refinement. This version aims to improve both the conceptual quality and practical applicability of the optimized rules [Click here to view system message and prompt](RQ3_version4.pdf).
