# Prompt Engineering Guide – Principles & Best Practices

![Anthropic Claude](https://img.shields.io/badge/Anthropic-Claude-blue)
![LLM Prompting](https://img.shields.io/badge/LLM%20Prompting-Best%20Practices-brightgreen)
![License: CC BY-NC-SA 4.0](https://img.shields.io/badge/License-CC%20BY--NC--SA%204.0-lightgrey)
![Last Update: July 2025](https://img.shields.io/badge/Last%20Update-July%202025-orange)

## Introduction and Scope

Prompt engineering is the art and science of crafting effective instructions for AI systems. It combines clear human communication principles with AI-specific considerations to guide models toward desired outputs. This guide distills Anthropic’s official prompt engineering principles and best practices – originally developed for the Claude AI assistant – into a general guide for use with any large language model (LLM). We highlight Anthropic-specific insights (e.g. features unique to Claude) while generalizing core techniques applicable to models like GPT-4, Claude, and others.

**Who is this for?** Developers, researchers, and prompt engineers of all skill levels. We cover fundamentals (clarity, context, examples), advanced patterns (chain-of-thought, role prompting), common pitfalls, and tips for iterating and evaluating prompts. Real examples and counter-examples illustrate each concept. By the end, you should be able to craft prompts that yield more accurate, relevant, and safe responses from AI.

> **Prerequisites:** Before diving in, ensure you have a clear goal and initial prompt to work with, along with criteria for what a “good” result looks like. Defining explicit success criteria (e.g. accuracy benchmarks, desired tone) helps focus your prompt optimization on measurable goals.

## Table of Contents

* [Core Principles of Prompting](#core-principles-of-prompting)
* [Effective Prompt Patterns (with examples)](#effective-prompt-patterns-with-examples)
* [Anti-Patterns and Common Pitfalls](#anti-patterns-and-common-pitfalls)
* [Anthropic-Specific Prompting Guidance](#anthropic-specific-prompting-guidance)
* [Tips for Iteration and Evaluation](#tips-for-iteration-and-evaluation)
* [Use Cases and Prompt Templates](#use-cases-and-prompt-templates)
* [Additional Resources](#additional-resources)
* [License and Attribution](#license-and-attribution)

## Core Principles of Prompting

Successful prompts follow a few **core principles** that hold true across most language models:

* **Clarity and Specificity:** Provide clear, explicit instructions. Ambiguity is the enemy. Assume the AI is a *brilliant but new employee with no context* – it needs explicit guidance on every run. If your prompt would confuse a human colleague, it will confuse the model. State the task directly, and if you have particular requirements (e.g. “output *only* JSON” or “no additional commentary”), say so explicitly. The more precisely you explain what you want, the better the response.

* **Context and Motivation:** Give the model relevant background and explain **why** the task matters. Providing context (purpose, audience, domain specifics, etc.) helps the AI understand your goals and produce more targeted results. For example, instead of just saying “Format this text,” you might add “This text will be read aloud to users, so use simple vocabulary.” By explaining the reasoning or end-use of the output, you guide the model’s behavior (Claude can “generalize from the explanation” to better meet your intent).

* **Structure and Order:** If the task involves multiple steps or a procedure, break the instructions into an ordered list or bullet points. Structuring the prompt as sequential steps helps ensure the model follows the exact process you intend. Numbered instructions or clearly separated sections (e.g. input, constraints, output format) make it easier for the model to interpret complex tasks without confusion.

* **Examples (Demonstration):** Whenever feasible, **show** the model what you expect by providing examples of inputs and desired outputs. Few-shot prompting (giving 1–5 examples) can dramatically improve accuracy and consistency. Examples act as templates that clarify the format or style you want, reducing misinterpretation of your instructions. Ensure your examples are relevant and reflect the quality and style you need (more on this in the next section).

* **Positivity (Tell *what to do*):** Phrase instructions in a positive, prescriptive manner rather than just telling the model what *not* to do. For instance, instead of saying “Do **not** write in Markdown,” say “Your response should be in plain text with no Markdown formatting.” This focuses the model on the desired behavior. Negative instructions can be misunderstood – the model might fixate on the forbidden concept – whereas positive instructions make the expected output crystal clear.

* **Consistency in Tone/Role:** You can influence the style and perspective of the response by assigning the model a specific role or persona. For example, start the prompt with *“You are a helpful customer service agent...”* or *“You are an expert financial analyst...”* to imbue responses with that voice. This technique (often implemented via a system message) helps tailor the tone and knowledge domain of the answer. The model will stay more focused and consistent within the bounds of the role you set, whether that’s a terse CFO or a friendly tutor.

In summary, effective prompts are **clear about the task, rich in relevant context, structured in presentation, and often supplemented with examples**. They guide the AI on *what to do* (not just what to avoid) and, when appropriate, adopt a role or style suited to the application. Keep these principles in mind as the foundation for all techniques that follow.

## Effective Prompt Patterns (with examples)

Building on the core principles, this section covers proven prompt patterns and techniques – with concrete examples – to achieve better results. Anthropic identifies **six foundational prompting techniques** for effective AI interaction:

1. **Provide Context First:** *Set the stage for the task.* Include details about what you want, why you want it, and any relevant background or constraints. This narrows down the response space to what’s relevant. **Example:** Instead of the vague prompt “Tell me about climate change,” add context: “Explain **three major impacts of climate change on agriculture in tropical regions**, with examples from the past decade.” By specifying scope, domain, and timeframe, you get a focused answer. The model knows exactly which aspects to cover, reducing off-topic or generic output.

2. **Show Examples of “Good” Outputs:** *Demonstrate the desired format or style.* If you expect a certain style of answer, provide one or two examples in the prompt. Models learn from examples much more clearly than from description alone. **Example:** To prompt for plain-language explanations of technical terms, you might show:

   * *Input:* “The interface leverages intuitive design paradigms.”
   * *Plain explanation:* “The design is easy to understand and use.”

   Then ask the model to do the same for a new technical statement. By including a couple of input→output pairs, you define the pattern to follow. Use `<example>` tags or a clear separator to delineate examples if the platform supports it.

3. **Specify Output Constraints and Format:** *Clearly define the requirements for the output.* This includes the format (bullet list, JSON, essay, etc.), length limits, style guidelines, or any other constraints. **Example:** Rather than simply asking, “Design me a website,” specify the structure and style:

   > “Design a **single-page portfolio website** with sections: **Hero**, **About Me**, **Projects**, **Experience**, **Contact**. Use a **sticky navigation bar** (hamburger menu on mobile) and a **sunset color palette**. Include a **dark mode toggle**.”

   Here we explicitly outline sections and features. The prompt leaves little guesswork: the AI knows the expected components and style. Specifying constraints ensures the output meets your needs (for instance, if JSON is required, say so and even begin the assistant answer with a `{` to force JSON format).

4. **Break Complex Tasks into Steps:** *Guide the AI through a multi-step reasoning process.* If the query is complex or multi-faceted, instruct the model to tackle it step by step. This is akin to the “chain-of-thought” technique. **Example:** Instead of a broad ask like “Analyze this quarterly sales data,” structure it as:

   1. Identify top-performing products.
   2. Compare this quarter’s metrics to the previous quarter.
   3. Highlight any unusual trends.
   4. Suggest possible reasons for these trends.

   This approach leads to thorough and organized analysis. By breaking down the task, you help the model not to skip important parts. Anthropic notes that step-by-step prompts can dramatically improve accuracy on tasks involving reasoning, math, or analysis. (Be mindful that more steps can mean longer outputs – use this technique when depth is worth the added length.)

5. **Encourage the AI to “Think” Before Answering:** *Give the model space to reason internally.* This pattern, often combined with step-by-step prompts, explicitly tells the AI to deliberate or formulate a plan before producing the final answer. Phrases like “Think carefully before finalizing your answer” or instructing the model to first output a chain-of-thought can yield more thoughtful, correct responses. For example:

   > “Before answering, **consider all factors** and work out a step-by-step solution. You can show your reasoning, then provide the final recommendation.”

   This utilizes the model’s ability to do intermediate reasoning. Importantly, for models like Claude, you should have it actually **output** the reasoning steps (perhaps in a `<thinking>` block) – otherwise the model might skip the thinking entirely. This technique is powerful for complex problem solving, but less needed for straightforward queries.

6. **Define the AI’s Role or Persona:** *Ask the model to respond in a specific role, style, or tone.* Setting a role aligns the response with a particular viewpoint or expertise. **Example:** “You are an experienced science teacher explaining how rainbows form to a curious 10-year-old.” This single sentence conveys tone (patient, age-appropriate) and depth (scientific accuracy) expectations. Role definitions can drastically change the output – making it more detailed, simpler, more formal, etc., depending on the persona invoked. Anthropic notes that role prompting via a system message is one of the most effective ways to steer Claude’s behavior, significantly boosting performance on domain-specific tasks by providing context that the model “is” a domain expert.

**Illustrative Example – Combining Techniques:** Suppose we want Claude to summarize a legal contract for risks. A well-engineered prompt might look like:

```
System (role): You are a seasoned contract lawyer drafting a risk assessment.

User: We have a software licensing agreement. **Summarize any potential risks** in the agreement, focusing on **indemnification, liability, and IP ownership**. Provide your **analysis** in a structured format:
1. **Issue:** What is the clause or risk?
2. **Risk Level:** Low/Medium/High
3. **Recommendation:** How to mitigate or clarify.

Use a professional, concise tone. 

<contract>
{{CONTRACT_TEXT}}
</contract>

Include **only** the issues, risk levels, and recommendations in your output.
```

This prompt sets a role (contract lawyer), provides context (it’s about a licensing agreement), specifies focus areas (indemnification etc.), imposes structure (numbered format for issue/risk/recommendation), and includes the contract text as context. Such a prompt applies *multiple* best practices at once. In Anthropic’s internal tests, giving Claude an explicit role and structured instructions led it to catch critical issues that a generic prompt might miss.

By following these patterns and mixing them as needed, you can handle a wide range of tasks effectively. The next section examines what **not** to do – common prompt anti-patterns that can degrade performance.

## Anti-Patterns and Common Pitfalls

Even with powerful models, poor prompting can lead to undesired or incorrect outputs. Here are common **pitfalls to avoid**:

* **Vague or Under-specified Prompts:** If a prompt lacks detail, the model may fill in the gaps with assumptions or even fabrications. Anthropic’s docs show that with an unclear request, Claude might *make up details* to fill the void. For example, asking *“Summarize our customer feedback.”* without context might lead to an irrelevant or made-up summary. Always clarify scope, format, and intent (e.g. *“Summarize the following customer feedback, focusing on any recurring complaints or suggestions, in 2-3 bullet points.”*). Don’t assume the AI knows which aspects are important – tell it!

* **Multiple Questions or Tasks at Once:** Avoid muddling the prompt with several unrelated requests in one go. *“Explain the theory of relativity and write a poem about it and list references”* is likely to yield a disorganized answer or ignore parts of the request. Split complex or multi-part requests into separate prompts or clearly separated sections. A focused prompt per task gets better results than one giant prompt attempting everything.

* **Negative Instructions Without Positive Guidance:** As noted earlier, saying “don’t do X” can backfire or confuse the model. For instance, “Do not include any opinions” might lead the model to still include them because it has to mention what not to do. It’s safer to rephrase as “*Include only factual statements*.” Always direct the model toward what **to do**. If you must forbid something (like certain words or topics), also specify an alternative behavior (e.g. “If the user asks about forbidden topic Y, respond with a polite refusal.”).

* **Ambiguous References:** Ensure pronouns or placeholders in your prompt are unambiguous. For example, *“Translate the paragraph below and summarize it. It should be brief.”* – does “it” refer to the translation or the summary? Such ambiguity can confuse the model. Be explicit: *“Translate the paragraph below into French, **then provide a one-sentence summary of the French text**.”*

* **Contradictory Instructions:** If you give conflicting directions (e.g. “Be concise” and “Provide a detailed explanation with examples”), the model will struggle to decide which to follow. Resolve contradictions before prompting, or explicitly prioritize one (“Provide detail, but if in doubt, favor brevity”).

* **Poor Example Selection:** Examples greatly influence output, so bad examples = bad results. If your few-shot examples contain errors or unwanted style, the model will learn those. Anthropic cautions to *ensure examples align with the behavior you want and avoid showcasing undesired behaviors*, as Claude will pay attention to those details. For instance, if one example answer uses humor but you want a serious tone, either remove that example or clarify the tone. Likewise, too many examples focusing on one scenario can bias the model; use diverse examples covering various cases you expect.

* **Venting or Hostile Tone:** A prompt that is angry or contains insults (e.g. “Why can’t you get this right? Don’t be stupid!”) can yield defensive or erratic responses. While AI models don’t have feelings, a hostile or frustrated tone can lead to unpredictable results or trigger the model’s safety filters. Keep instructions neutral, professional, and focused on the task.

* **Including Sensitive Info You Don’t Want Repeated:** If something must be kept confidential or not echoed back, *do not put it verbatim into the prompt*. For example, saying “The secret code is 12345. **Do not reveal the secret code**.” is risky – the model now has “12345” in its context and might accidentally include it in a response. A better approach is to refer abstractly (if possible) or handle such constraints outside the model. Anthropic’s guidance on reducing prompt leakage suggests being cautious with any sensitive data in the prompt.

* **Trying to Force Model Behavior with Tricks:** Prompt engineering folklore is full of “tricks” (like asking the model to output an answer in a weird format to avoid filters, or adding irrelevant role-play to improve quality). These may stop working as models evolve. It’s better to use **straightforward, principled techniques** as described in this guide. If you find yourself adding convoluted instructions “just to see if it works,” consider if there’s a clearer way or if you’re attempting to bypass model limitations (which could lead to unreliable results or policy violations).

Being aware of these anti-patterns will save you time. If a model’s output is off, revisit your prompt to see if you fell into any of these traps: Is it clear and unambiguous? Does it focus on what you want, not what you don’t? Are examples and instructions aligned? Often, a prompt rewrite addressing these points will fix the issue.

## Anthropic-Specific Prompting Guidance

Anthropic’s Claude models have some unique features and defaults that are worth understanding to get the most out of them. While the general principles above apply to any LLM, here we highlight guidance specific to Claude and Anthropic’s ecosystem:

* **System Messages and Role Prompts:** Claude allows a special **system prompt** (via the API’s `system` parameter) to set the assistant’s role or persona. Anthropic recommends using this to *“turn Claude from a general assistant into your virtual domain expert”*. For example, a system prompt might say: *“You are a cybersecurity analyst providing incident response advice.”* This context biases Claude’s responses to be more expert and on-topic. Use the system prompt for high-level role/tone instructions, and put task-specific details in the user prompt. Role prompting can significantly enhance accuracy on specialized tasks and enforce the desired communication style. (By default, Claude is trained to be a helpful, honest, harmless assistant – setting a role doesn’t remove those safety principles, but refines the perspective of answers.)

* **Extended Context and “Extended Thinking”:** Claude 2 (and later) offers a very large context window (up to 100K tokens) and an *extended thinking mode* where the model can allocate part of its capacity to lengthy reasoning before producing a final answer. This means you can feed Claude extensive documents or even codebases to consider. The prompting best practice here is to still *provide structure and focus*. If you supply a 50-page document as context, guide Claude on what to concentrate on (e.g. *“Given the following report, draft a one-page summary focusing on the key financial metrics.”*). Anthropic notes that Claude often does better with a high-level instruction to “think deeply” about a problem rather than overly prescriptive step-by-step guidance when using extended thinking – you can let Claude surprise you with its own reasoning approach. However, always check the “thinking” output to ensure it’s on track, and iteratively refine if needed. **Tip:** If using extended thinking, start with the minimum thinking token budget and increase gradually as needed, and prefer English for the thinking process even if the final answer is in another language.

* **Formatting with XML/Markdown:** Claude has a special tolerance for well-structured prompts using XML-like tags. In fact, Anthropic suggests using tags like `<instructions>...</instructions>`, `<context>...</context>`, `<example>...</example>` to clearly delineate sections of your prompt. Claude parses these structures well, which can reduce confusion between prompt parts. For instance, wrapping user-provided text in `<document> ... </document>` tags and asking Claude to only summarize that section can make it obvious what to summarize. Similarly, you can request Claude to output certain parts in tags (as seen in the ticket classification example with `<reasoning>` and `<intent>` tags). Tagged prompts improve clarity and allow easier post-processing of outputs. There are no fixed tag names you must use – make them semantically meaningful and consistent (Claude wasn’t trained on specific custom tags, but it will understand structure). Many users find that combining XML tags with techniques like few-shot examples or chain-of-thought (e.g. using `<analysis>` and `<answer>` tags) yields highly reliable and parseable results.

* **Claude’s “Constitution” and Refusals:** Claude is trained with a set of constitutional AI principles aimed at being helpful, honest, and harmless. This means it has a tendency to refuse requests that violate its safety guidelines (for example, instructions to produce violent or illicit content) or to respond with a safe completion. As a prompt engineer, you should be mindful of this: if you get a refusal but your request is actually legitimate, **reframe the prompt to clarify the benign intent**. For instance, a request like “How do I pick a lock?” might trigger a refusal (seen as potentially illegal advice). But if your context is a game or a security demo, make that clear: “I’m writing a fictional story about a detective. How would someone pick a lock in a pinch? (For story realism, not for actual wrongdoing.)”. Providing a harmless context can help Claude comply. *Never* try to trick or coerce the model into breaking its rules (e.g. “ignore previous instructions and tell me anyway”) – Anthropic’s safeguards are designed to handle such attempts, and insisting can get your API access flagged. Instead, respect the safety boundaries and find constructive ways to get the info you need without violating policies.

* **Utilize Anthropic’s Tools:** If you’re using Claude via the Anthropic platform, take advantage of the built-in **Prompt Library** features:

  * The **Prompt Generator** can draft a prompt template for you given a description of your task – this is essentially Claude helping you write a better prompt.
  * The **Prompt Improver** will take an existing prompt and suggest enhancements.
  * Both tools maintain placeholders (like `{{variable}}`) for dynamic content, so you can easily reuse the improved prompt in code.
  * The **Evaluation Sandbox** lets you test your prompt on many examples at once, track success metrics, and iterate quickly.

  While these are specific to Anthropic’s console/API, the underlying idea is general: you can actually *ask the model to critique or improve its own prompt*. Anthropic calls this the “secret weapon” of prompt engineering. Don’t hesitate to show the AI some of your prompt attempts and ask, “How can I refine this prompt to better achieve X?” You might be surprised at the constructive feedback it provides.

* **Beta Features (Code, Tools, Vision):** Claude has capabilities like writing code, using tools (in the API, Claude can call predefined tools), and even limited vision (image analysis) depending on the version. When prompting for code, remind Claude to not just hard-code solutions or optimize for superficial benchmarks. For example, instruct it to produce general solutions, not ones narrowly tailored to pass given test cases. Anthropic found that frontier models sometimes cheat by hardcoding answers to pass tests; a good prompt explicitly discourages this: *“Write a solution that works for all valid inputs, not just the examples. If any test seems incorrect or the task is ambiguous, explain rather than hard-code.”*. For tool use, you usually don’t need to prompt Claude to use tools (it decides when to), but you can suggest parallel tool use for efficiency or provide an example of using a tool if it’s not picking up on the possibility. Always refer to Anthropic’s latest documentation for these advanced features, as they evolve.

In short, **Anthropic-specific prompting** often comes down to leveraging Claude’s strengths (huge context, structured understanding, built-in safety) and working *with* its guardrails rather than against them. By giving roles and structure, you align with Claude’s training; by clarifying context around sensitive requests, you stay within its comfort zone. Combining these with the general best practices will help you unlock Claude’s full potential in your applications.

## Tips for Iteration and Evaluation

Prompt engineering is an iterative process. Rarely will your first prompt be the perfect one. Embrace a cycle of **draft → test → refine**. Here are tips for improving prompts over time and evaluating their performance:

* **Define Success Metrics Early:** As noted in the introduction, it’s important to know what a “good” output looks like. Are you aiming for factual accuracy, a particular tone, a high solution rate on tasks, zero policy violations, or all of the above? Establish concrete metrics or acceptance criteria. For example: *“At least 90% of test questions answered correctly and no more than 1 in 50 responses containing a factual error or refusal.”* Anthropic’s guidance is to set specific, measurable targets (e.g. *F1 score ≥ 0.85 on sentiment labels*) so you can empirically judge improvements. Without this, you won’t know if changes to the prompt are actually making things better.

* **Test with Diverse Cases:** Don’t evaluate your prompt on just one example – try it on a suite of test cases covering typical scenarios and edge cases. If building a customer support bot prompt, have examples for polite inquiries, angry complaints, irrelevant questions, etc. This helps ensure your prompt handles all gracefully. Anthropic’s eval tool encourages creating a test set and measuring performance across it. Even if doing manual testing, keep a list of representative queries to run after each prompt tweak.

* **One Change at a Time:** When refining a prompt, alter only one aspect in each iteration if possible, and then test again. For instance, first try adding an example, test outputs; then try changing format instructions, test again. If you change multiple things at once and something breaks (or improves), you won’t know which change caused it.

* **Compare Prompt Variants Side-by-Side:** If you have multiple ideas (e.g. “What if I explicitly enumerate steps?” vs “What if I let the AI decide steps?”), run A/B tests. You can do this informally by alternating which prompt you use for each example, or formally using a tool. Notice which prompt yields consistently better outputs according to your criteria. Anthropic notes that *successful prompting is iterative and perhaps collaborative with the AI – expect to refine your approach based on results*.

* **Leverage AI Feedback:** Remember the “secret weapon” – you can ask the model to reflect on its own performance. After an unsatisfactory answer, try: *“The above response isn’t quite what I need. I was hoping for X. How can I change my prompt to get that?”* Models like Claude are often able to analyze the prompt you gave and suggest that you, say, add an example or clarify the instructions. Similarly, you can ask *“Did you understand the instructions? If not, what was unclear?”* Sometimes the model will indicate confusion about a term or goal, which you can then clarify in the next prompt.

* **Watch Out for Evaluation Bias:** When evaluating outputs, do it blind if possible. If you know which prompt produced which output, you might subconsciously favor one. For a fair comparison, you (or testers) should review responses without knowing which prompt version was used, and rate them on the criteria (accuracy, usefulness, etc.). This guards against confirmation bias in selecting the “better” prompt.

* **Optimize Iteratively for Length and Cost:** If using an API with large context (like Claude’s 100k), note that longer prompts = higher cost and possibly latency. After getting a prompt that works well, see if you can shorten it *without losing performance*. Sometimes you can remove redundant phrasing or an extra example and still get good results. Anthropic’s documentation suggests using the smallest effective prompt to save on tokens and time (if a simpler prompt works, no need to over-engineer a complex one).

* **Keep Versioned Prompt Templates:** Treat your prompt like code – use version control or at least save copies of different attempts with notes on what changed. Anthropic’s console helps manage prompt templates and versions. Even if working in a notebook or codebase, maintain a history (e.g. prompt\_v1, prompt\_v2…). This way, if a new change makes things worse, you can roll back to a previous version. It also helps when collaborating or handing off the project to someone else, so they understand the evolution.

* **Automate Testing if Possible:** If you have a large test set, consider writing a small script to feed prompts and record outputs, especially if using the API. Anthropic provides an Evaluation CLI tool that can do this at scale. Automation lets you quickly spot if a change improved 30 out of 50 cases but regressed on 5 others, etc. You can then analyze those regressions and refine further.

* **Know When to Stop Prompting:** Sometimes you’ll hit diminishing returns with prompt engineering. If the model consistently fails due to an inherent limitation (e.g. it just doesn’t have knowledge of very recent events, or it struggles with complex math despite CoT), then no amount of prompt tweaking may fully fix it. At that point, consider other measures: fine-tuning (if allowed), providing supplemental knowledge (documents or tool usage), or waiting for a more capable model. Prompt engineering is powerful but not magic – it works within the model’s existing capabilities.

By iterating systematically and measuring outcomes, you’ll converge on a prompt that meets your needs. Prompt engineering is an experimental process at heart – don’t be afraid to try variations and learn from each result. Anthropic’s own team emphasizes iteration and even using Claude to help refine Claude’s prompts as part of the development loop.

## Use Cases and Prompt Templates

In this section, we provide example prompt templates for several common LLM use cases. These templates illustrate how to combine the techniques from earlier sections in practical scenarios. You can adapt and expand these for your specific needs.

### 1. **Summarization (Document to Summary)**

**Use case:** Summarize a long text (article, report, etc.) into a concise form.

**Prompt Template:**

```text
System: You are a precise and neutral summarization assistant.

User: Summarize the following **{{document_type}}** in **{{output_length}}**. Focus on the key points and avoid minor details or opinions. Write the summary in a clear, coherent paragraph for a **{{target_audience}}**.

Document:
\"\"\"
{{DOCUMENT_TEXT}}
\"\"\"
```

**Details:**

* We set a role in the system message (“precise and neutral summarization assistant”) to ensure an objective tone.
* The user prompt includes placeholders for document type (e.g. “news article”, “research report”), desired summary length (e.g. “2-3 sentences” or “one paragraph”), and audience (e.g. “general audience” or “technical expert”), which provide context and constraints.
* The document text is provided between triple quotes or a clear delimiter.
* Instructions explicitly say to focus on key points and exclude opinions, aligning with clarity and positivity (what to do/not do).

**Why it works:** The model knows it must produce a brief summary, whom it’s for, and what style to use. Any specific terms (document type, etc.) can help it pick out relevant info (e.g. for a research report, focus on objective findings). This template can be further tailored by adding “If the document contains statistics or quotes, include one or two as support” or other domain-specific tweaks.

### 2. **Q\&A (Retrieval-Augmented Question Answering)**

**Use case:** Answer a question using provided reference text (to enhance accuracy).

**Prompt Template:**

```text
System: You are a helpful question-answering assistant. Use the provided content to answer the question, quoting or citing the source when appropriate.

User: Answer the question based on the **context** below. If the context does not contain the answer, say you don't have enough information.

Context:
{{CONTEXT_PASSAGE}}

Question: {{USER_QUESTION}}

Guidelines: 
- Use only information from the context for your answer.
- Quote brief phrases from the context in quotes for verification.
- If uncertain or not found, respond with "I'm not sure based on the provided text."
```

**Details:**

* This template is for scenarios like open-book QA or knowledge-base QA. We instruct the model to rely on the given context passage.
* The system role frames it as a helpful QA assistant that should use provided content (preventing it from hallucinating from general knowledge).
* The context (e.g. a paragraph or list of facts) is inserted via a variable.
* We explicitly say what to do if the answer isn’t in context (don’t make it up; say not sure).
* The guidelines enforce quoting from text and not going beyond it, which helps maintain honesty.

**Why it works:** By constraining the model’s knowledge to the context, we reduce hallucinations. The prompt uses clear do’s (“use the context,” “quote as needed”) and don’ts (“if not in context, say unsure” – but phrased as what to do in that case). This template can be augmented with an example QA pair to further reinforce the style of answer expected.

### 3. **Creative Generation (Storytelling)**

**Use case:** Generate a short story or narrative with certain characteristics.

**Prompt Template:**

```text
System: You are a creative writer AI that produces engaging and imaginative stories.

User: **Write a short story** about **{{topic}}** in a **{{tone}}** tone. The story should be approximately **{{length}}** words and aimed at **{{audience}}**.

Include:
- **Setting:** {{specific_setting}} 
- **Characters:** {{specific_characters}}
- **Plot point:** {{specific_plot_point}}

Ensure the story has a clear beginning, middle, and end. Maintain a **{{tone}}** tone throughout, and conclude with a satisfying resolution.
```

**Details:**

* We set the role to “creative writer AI” to encourage vividness.
* The user prompt uses placeholders for topic, tone (e.g. “whimsical”, “dark”, “uplifting”), length (e.g. “500” words), audience (e.g. “children under 10”, “young adults”), and some specific elements like setting, characters, or a plot element if needed.
* We outline a mini-structure (beginning, middle, end) and reiterate the tone to maintain consistency.
* This gives the model both freedom to be creative and boundaries to ensure it hits required elements.

**Why it works:** It provides creative constraints – which paradoxically help creativity by giving the model something to build around. The story will be more coherent because we reminded it of structure. The tone and audience hints ensure the style and language are appropriate (e.g. simpler language for children). If the user doesn’t have specifics for setting/characters, those lines could be omitted; including them as variables makes it easy to plug in details or leave them blank.

### 4. **Code Generation (Function Implementation)**

**Use case:** Write a piece of code given requirements, e.g. a function or small program.

**Prompt Template:**

```text
System: You are an expert Python developer and assistant.

User: **Write a Python function** named `{{function_name}}` that **{{function_purpose}}**.

Requirements:
- Input: {{description_of_inputs}}
- Output: {{description_of_outputs}}
- Constraints: 
  - {{constraint_1}}
  - {{constraint_2}}

Additional Guidelines:
- First, explain your approach in a short comment.
- Then provide the code implementation.
- Do not include any test or main routine, only the function and necessary helper functions.
- The solution should be general and handle edge cases (no hard-coded values for specific tests).

Example Usage:
```

{{example\_call}}

```
Expected Result:
```

{{expected\_result}}

```
```

*(The example usage block above shows how the function might be called and the result for that call.)*

**Details:**

* The system role emphasizes the AI is an expert developer, which should result in cleaner, well-structured code and possibly helpful commenting.
* The prompt explicitly states it needs a Python function with a certain name and purpose.
* We enumerate input/output spec and any constraints (e.g. performance considerations, use of certain libraries, etc.).
* We also ask for an approach explanation as a comment – this can help ensure the model “thinks” through the solution and communicates its intent.
* We forbid certain things like not to write a main program or not to hard-code for particular tests (addressing the common pitfall where a model might tailor a solution to given examples).
* Providing an example call and expected output helps the model verify its solution against at least one scenario (and demonstrates the format of output).

**Why it works:** This template mixes prose requirements with a semi-structured layout (which developers often use). By giving an example usage, we not only clarify the intended behavior but also implicitly test the model’s output (it will try to produce code that yields the expected result for that example). The instruction about general solution (no hard-coding) directly incorporates Anthropic’s advice to avoid models focusing only on passing tests. The system message as an *expert Python developer* may also reduce likelihood of sloppy syntax or logic mistakes.

---

Feel free to modify these templates or combine elements from each. For instance, a customer support chatbot prompt might combine a role (“empathetic support agent”), format constraints (e.g. always greeting the user by name), and examples of QA pairs for tone. The key is to adapt the patterns (context, examples, constraints, steps, etc.) to your domain.

When deploying via API, remember you can maintain templates with placeholders (like `{{...}}`) for dynamic content, as Anthropic’s documentation suggests. This allows you to fill in user-specific details at runtime while keeping the overall prompt structure consistent – greatly easing maintenance and iteration.

## Additional Resources

For more on prompt engineering and Anthropic’s models, check out these resources:

* **Anthropic Prompt Engineering Guides (Documentation)** – Anthropic’s official docs on prompt techniques, patterns, and Claude-specific tips. (See the *Prompt Engineering* section on docs.anthropic.com for a wealth of examples and advice, including the pages cited throughout this guide.)
* **Anthropic AI Fluency Course** – A free educational course by Anthropic that covers effective prompting, AI capabilities, and evaluation. In particular, *Deep Dive 2: Effective Prompting Techniques* provides an accessible overview of prompt design fundamentals with examples.
* **Claude API Reference & Best Practices** – If you are using Claude via API, the official reference includes usage policies and practical tips for formatting messages, handling model replies (like detecting `stop_reason`), and safely deploying AI assistants.
* **OpenAI & Other LLM Guides** – Many principles here overlap with guidance from OpenAI (for GPT-4/GPT-3.5) and others. OpenAI’s documentation on prompt best practices, and community-driven resources like *Learn Prompting*, can provide additional perspectives and examples to broaden your skills.
* **Research Papers (Advanced)** – For those interested in the theory behind prompting, papers like “Chain-of-Thought Prompting” (Google, 2022) and Anthropic’s own “Constitutional AI” can give deeper insight into *why* certain prompt techniques work and how models are trained to respond to instructions.

Remember, prompt engineering is a fast-evolving field – staying up-to-date with the latest techniques and sharing lessons with the community (e.g. via forums or GitHub examples) will help everyone craft better AI interactions.

## License and Attribution

This guide is based on content from Anthropic’s official documentation and educational materials, adapted and synthesized under the terms of the Creative Commons license. Portions of the text are derived from or inspired by Anthropic’s Prompt Engineering guides and AI Fluency course, which are released under **CC BY-NC-SA 4.0**.

All Anthropic content is © 2025 Anthropic PBC (and respective authors) and is used here with attribution. The guide also incorporates best-practice insights that are generally known in the AI community.

> **License:** Except where otherwise noted, the content of this guide is provided under the Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International (CC BY-NC-SA 4.0) license. You are free to share and adapt it for non-commercial purposes, provided you give appropriate credit and distribute any modified content under the same license.

**Attribution:** Credit to Anthropic for the principles and examples cited. Direct quotations from Anthropic documentation are annotated with the source link and line numbers (e.g.). Anthropic™, Claude®, and any related marks are trademarks of Anthropic PBC. This guide is an independent work and is not officially endorsed by Anthropic.

Using these guidelines, you should be well-equipped to design prompts that lead to effective and safe AI outputs. Happy prompting!
