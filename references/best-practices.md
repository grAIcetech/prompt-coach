# Prompting Best Practices Reference

Complete reference for all 12 practices from Claude's official prompting guide. Each entry includes: what it is, when to recommend it, a before/after example, and the canonical doc link.

---

## 1. Be Clear and Direct

**What it is:** Be specific about desired output format, length, constraints, and audience. Use numbered lists when order matters. Imagine showing your prompt to a colleague — would they know exactly what to produce?

**When to recommend:** The user's prompt is vague, ambiguous, or leaves critical details unspecified (format, length, audience, style, structure).

**Before/after example:**

> Before: "Make something nice for my presentation"
>
> After: "Create a 3-slide executive summary for our Q2 board meeting. Each slide should have a bold headline, 3 bullet points max, and a data callout. Audience: non-technical board members. Tone: confident but not salesy."

**Doc link:** https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/be-clear-and-direct

---

## 2. Add Context to Improve Performance

**What it is:** Explain the WHY behind your instructions, not just the WHAT. Claude generalizes better from understanding purpose than from rigid rules.

**When to recommend:** The user gives instructions (formatting rules, constraints, stylistic choices) without explaining the reason behind them.

**Before/after example:**

> Before: "Always use bullet points in responses"
>
> After: "Use bullet points because this will be pasted into a Slack channel where people skim quickly — dense paragraphs get ignored there."

**Doc link:** https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/be-clear-and-direct#provide-context

---

## 3. Use Examples Effectively

**What it is:** Provide 3-5 diverse, relevant examples showing the input-output pattern you want. Known as few-shot or multishot prompting. Wrap in `<example>` tags for clarity.

**When to recommend:** The user asks for classification, categorization, formatting, or any task where the desired output pattern isn't obvious from the instructions alone.

**Before/after example:**

> Before: "Categorize these support tickets as urgent, normal, or low priority"
>
> After: "Categorize these support tickets. Here are examples of how I'd categorize them:
> <example>
> Ticket: 'Site is completely down, no one can log in' → urgent
> </example>
> <example>
> Ticket: 'Button color looks slightly off on mobile' → low
> </example>
> <example>
> Ticket: 'Password reset email takes 10 min to arrive' → normal
> </example>
> Now categorize these tickets using the same logic: [tickets]"

**Doc link:** https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/multishot-prompting

---

## 4. Structure Prompts with XML Tags

**What it is:** Use tags like `<instructions>`, `<context>`, `<input>`, `<examples>` to separate distinct parts of your prompt. Helps Claude parse complex prompts without confusion.

**When to recommend:** The prompt contains multiple distinct inputs (data, instructions, context, constraints) all running together in a single block of text.

**Before/after example:**

> Before: "I need you to analyze this sales data, here are the numbers 100 200 300, and also check the competitor pricing from last quarter, and make a chart"
>
> After:
> ```
> <task>Analyze our sales data and compare against competitor pricing. Output a chart.</task>
> <our_data>100, 200, 300</our_data>
> <competitor_context>Pull competitor pricing from last quarter's report</competitor_context>
> <output_format>Bar chart comparing both, with a summary paragraph</output_format>
> ```

**Doc link:** https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/use-xml-tags

---

## 5. Give Claude a Role

**What it is:** Set a role or persona to focus Claude's behavior, tone, and expertise. Even one sentence in the system prompt helps anchor responses.

**When to recommend:** The user asks for content that would benefit from a specific expert perspective, but doesn't specify who Claude should "be" while writing it.

**Before/after example:**

> Before: "Write me a cover letter for a healthcare compliance position"
>
> After: "You are an experienced healthcare hiring manager who has reviewed 500+ compliance officer applications. Write a cover letter that would catch YOUR attention for a senior compliance position at a mid-size hospital system."

**Doc link:** https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/give-claude-a-role

---

## 6. Long Context Prompting

**What it is:** When working with long documents, put the document/data at the TOP and your question/instructions at the BOTTOM. Use `<document>` tags with metadata. Ask Claude to ground responses in direct quotes.

**When to recommend:** The user puts their question before a large block of text, or references a long document without structuring the prompt to put context first.

**Before/after example:**

> Before: "Here's my question: what are the key risks? Now here's the 50-page document: [long text]"
>
> After:
> ```
> <document source="Q2 Risk Assessment" pages="50">
> [full document text here]
> </document>
>
> Based on the document above, identify the top 5 risks. For each, quote the relevant passage and explain the business impact.
> ```

**Doc link:** https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/long-context-tips

---

## 7. Control Output Format

**What it is:** Tell Claude what to DO instead of what NOT to do. Use format indicators (XML tags, markdown templates) to show the exact structure you want. Match your prompt's style to your desired output style.

**When to recommend:** The user defines their request primarily through negatives ("don't do X, don't do Y, avoid Z") rather than stating what they actually want.

**Before/after example:**

> Before: "Don't use jargon. Don't make it too long. Don't use bullet points."
>
> After: "Write in plain language a non-expert would understand. Keep it under 200 words. Use flowing prose paragraphs — conversational, like you're explaining to a friend over coffee."

**Doc link:** https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/be-clear-and-direct#tell-claude-what-to-do

---

## 8. Leverage Thinking Capabilities

**What it is:** For complex reasoning tasks, use extended thinking or ask Claude to work through the problem step by step. Guide thinking with explicit instructions. Ask Claude to self-check its reasoning.

**When to recommend:** The user asks a complex multi-factor analysis, calculation, or decision that requires weighing several variables — and the prompt doesn't invite step-by-step reasoning.

**Before/after example:**

> Before: "Calculate the optimal pricing strategy considering our costs, competitor pricing, market positioning, and customer willingness to pay"
>
> After: "Calculate the optimal pricing strategy. Think through this step by step:
> 1. First, analyze our cost structure and minimum viable price
> 2. Then map competitor pricing and positioning
> 3. Factor in customer willingness-to-pay data
> 4. Identify the sweet spot considering all three
> 5. Stress-test: what breaks if costs rise 20%?
> Show your reasoning at each step before giving a final recommendation."

**Doc link:** https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/extended-thinking-tips

---

## 9. Chain Complex Prompts

**What it is:** Break large multi-step tasks into sequential steps when you need to inspect intermediate outputs. Pattern: generate → review → refine. Each step builds on the validated output of the previous one.

**When to recommend:** The user asks for multiple distinct deliverables in a single prompt, especially when later steps depend on earlier ones.

**Before/after example:**

> Before: "Research AI in healthcare, write a 10-page report, create an executive summary, and generate a slide deck from it"
>
> After: Break this into steps:
> "Step 1: Research AI in healthcare — give me an outline of key themes with sources"
> [review outline, then...]
> "Step 2: Expand that outline into a full report"
> [review report, then...]
> "Step 3: Write an executive summary of that report"
> "Step 4: Create a slide deck from the executive summary"

**Doc link:** https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/chain-prompts

---

## 10. Use Parallel Tool Calling

**What it is:** When multiple tool calls have no dependencies between them, structure your request so they can run in parallel rather than sequentially. This is faster and more efficient.

**When to recommend:** The user describes multiple independent operations (read these files, check these APIs, search these sources) in a way that implies sequential execution when parallel is possible.

**Before/after example:**

> Before: "Read file A, then read file B, then read file C, then tell me what they contain"
>
> After: "Read these 5 files simultaneously and give me a summary of each: [file list]. These are independent — you can read them all at once."

**Doc link:** https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/chain-prompts#parallel-processing

---

## 11. Provide Clear Success Criteria for Research

**What it is:** Define what constitutes a successful answer. How many sources? What depth? What format? What would make you say "this is exactly what I needed"? Encourage source verification.

**When to recommend:** The user asks for research, investigation, or analysis without specifying what "done" looks like — leaving Claude to guess at appropriate depth and breadth.

**Before/after example:**

> Before: "Find me information about HIPAA compliance changes"
>
> After: "Research HIPAA compliance changes from 2024-2025. I need:
> - At least 3 specific regulatory changes with effective dates
> - How each change affects small healthcare practices (under 50 employees)
> - Source each claim to an official HHS document or Federal Register entry
> - Flag anything that's proposed-but-not-final vs. already in effect
> Success = I can hand this to our compliance officer and she can act on it today."

**Doc link:** https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/be-clear-and-direct#define-success-criteria

---

## 12. Minimize Hallucinations

**What it is:** Instruct Claude to read files before answering questions about them. Never ask Claude to speculate about unseen code or data. Give explicit permission to say "I don't know" or "I need to check that first."

**When to recommend:** The user asks about specific file contents, code behavior, or data without first providing or instructing Claude to read the source material.

**Before/after example:**

> Before: "What does the function on line 42 of server.js do?"
>
> After: "Read server.js first, then explain what the function on line 42 does. If the file structure is different from what I described, tell me what you actually see rather than guessing."

**Doc link:** https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/be-clear-and-direct#reduce-hallucinations
