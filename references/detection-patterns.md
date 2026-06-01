# Detection Patterns

How to decide which best practice to recommend for a given user prompt.

## How Matching Works

1. **Specificity wins.** A narrow, highly-specific pattern always beats a broad one. Before declaring a match on any pattern, check whether a MORE SPECIFIC pattern further down the list is a stronger fit.
2. **Pre-scan for override conditions first.** Before walking the priority list, check the Override Rules below. If an override fires, it jumps directly to that practice — skip the normal priority scan.
3. **Then scan in priority order.** Walk the numbered priorities. Recommend the **first match** only, respecting each pattern's "Skip if" and "Yield to" clauses.

---

## Override Rules (Check These First)

These conditions are so specific that they override normal priority order. Check them before the priority scan.

**Override A — Negative Framing Override**
If the prompt contains **3 or more** "don't" / "never" / "avoid" / "no" instructions → match **Control output format (positive framing)** immediately, regardless of other signals. The density of negative constraints is the dominant issue.

**Override B — Chaining Override**
If the prompt requests **3 or more distinct deliverables** where later steps depend on earlier ones (e.g., "research X, write a report from the research, make slides from the report") → match **Chain complex prompts** immediately. The multi-step dependency chain is the dominant issue, even if role or XML tags also apply.

**Override C — Thinking Override**
If the prompt involves a **multi-factor quantitative decision** with specific numbers/data points that require weighing against each other (e.g., pricing strategy with cost data, competitor prices, and survey ranges) → match **Leverage thinking capabilities** immediately. The reasoning complexity is the dominant issue, even if the data could also benefit from XML tags.

**Override D — Context/Purpose Override**
If the prompt is **entirely formatting instructions** (bullet points, sentence limits, header rules, etc.) with **no actual task or topic**, and the instructions lack any rationale → match **Add context to improve performance** immediately. The absence of "why" is the dominant issue, not vagueness.

---

## Priority 1: Vague or Underspecified Instructions

**Practice:** Be clear and direct

**Signals:**
- Prompt uses subjective qualifiers without definition: "nice," "good," "professional," "clean," "better"
- Missing critical specs: no format, length, audience, or structure mentioned
- The prompt could produce wildly different outputs depending on interpretation
- Request is a single short sentence for a complex deliverable (e.g., "Make me a presentation")

**Yield to:** If the prompt's main problem is negative framing (3+ don'ts), yield to positive framing. If the prompt is pure formatting rules with no task, yield to "Add context." If the prompt is an open-ended research request ("find information about," "look into," "what do you know about"), yield to "Provide clear success criteria" — lack of scope on research is a success-criteria problem, not a vagueness problem.

**Skip if:** The user is in an ongoing conversation where prior context fills in the gaps. Also skip if the prompt has clear specifics (audience, format, data) but is missing something else (like a role or examples) — that's not a "vague" prompt.

---

## Priority 2: No Examples for Format-Sensitive Tasks

**Practice:** Use examples effectively

**Signals:**
- Task involves classification, categorization, labeling, or scoring
- User wants output in a specific pattern/format but describes it in words rather than showing it
- Task is about tone matching, style replication, or "make it like X"
- Any task where seeing 2-3 examples would eliminate ambiguity faster than more words

**Skip if:** The format is truly obvious (e.g., "write a haiku" — the format is self-evident).

---

## Priority 3: Multiple Inputs Mashed Together

**Practice:** Structure prompts with XML tags

**Signals:**
- Prompt contains data/numbers mixed into instruction text
- Multiple distinct inputs (context, data, constraints, questions) in a single undifferentiated block
- Comma-separated or run-on instructions covering different concerns
- The user says "and also" or "oh and" to tack on separate concerns mid-prompt

**Yield to:** If the data is there to support a multi-factor quantitative decision (pricing, budgeting, trade-off analysis), yield to "Leverage thinking capabilities" — the reasoning challenge outweighs the structural one. If the prompt also has 3+ distinct deliverables in a dependency chain, yield to "Chain complex prompts."

**Skip if:** The prompt has only one input type or is short enough that structure adds no clarity. Also skip if the data is embedded but the real issue is that the prompt asks for a complex multi-step reasoning task — XML tags help, but thinking guidance helps more.

---

## Priority 4: No Role or Persona Specified

**Practice:** Give Claude a role

**Signals:**
- User asks for expert-level content (legal, medical, technical, creative) with no persona context
- Content creation tasks: emails, letters, proposals, articles
- Tasks where tone/perspective matters: feedback, critiques, reviews
- User says "write as if" or "pretend" (they're almost there — just need the explicit role framing)

**Yield to:** If the prompt also has 3+ negative constraints ("don't X, don't Y, don't Z"), yield to positive framing — the negative framing is a more impactful fix. If the prompt has 3+ distinct deliverables in a dependency chain, yield to chaining. If the prompt involves a multi-factor quantitative decision with specific numbers, yield to thinking.

**Skip if:** The task is purely mechanical (data transformation, file operations, calculations) where a role adds no value. Also skip if another more specific pattern (positive framing, chaining, thinking) is a stronger match for the prompt's dominant weakness.

---

## Priority 5: Negative Framing

**Practice:** Control output format (positive framing)

**Signals:**
- Multiple "don't" / "never" / "avoid" / "no" instructions (3+ is a strong signal)
- Constraints defined by exclusion rather than inclusion
- More words spent on what's unwanted than what's wanted
- Pattern: "Don't X. Don't Y. Don't Z." without corresponding "Do A. Do B."

**Skip if:** There's only a single negative alongside positive instructions (that's normal and fine).

---

## Priority 6: Complex Multi-Step Task in One Prompt

**Practice:** Chain complex prompts

**Signals:**
- Request includes 3+ distinct deliverables (e.g., "research X, write a report, make slides, draft an email")
- Later steps depend on earlier ones (the slides depend on the report content)
- The user would want to review/adjust intermediate outputs before proceeding
- Task would take a human multiple work sessions to complete

**Skip if:** The steps are simple and independent (e.g., "rename these 3 files" — that's one atomic task, not a chain).

---

## Priority 7: Research/Analysis Without Success Criteria

**Practice:** Provide clear success criteria

**Signals:**
- Open-ended research requests: "find information about," "look into," "what do you know about"
- No definition of scope, depth, format, or what "good enough" looks like
- Analysis tasks without specifying what decisions the analysis will inform
- The user would have trouble knowing when the task is "done"

**Skip if:** The request is specific enough to have natural boundaries (e.g., "what's the population of France" has a clear answer).

---

## Priority 8: Long Context with Query on Top

**Practice:** Long context prompting

**Signals:**
- User states their question, THEN pastes a large block of text
- Pattern: "My question is X. Here's the document: [long text]"
- References to "the above" or "the following" where the document hasn't been provided yet at the query point
- Large document (>500 words) with instructions at the beginning rather than end

**Skip if:** The document is short (<500 words) — ordering matters less for short context.

---

## Priority 9: Complex Reasoning Without Thinking Guidance

**Practice:** Leverage thinking capabilities

**Signals:**
- Multi-factor decision or analysis (pricing strategy, architecture choice, risk assessment)
- Math or logic that requires multiple steps
- "Weigh the pros and cons" or "consider all factors" without structured approach
- Tasks where showing reasoning is as important as the answer

**Skip if:** The task is straightforward or the user explicitly asks for just the bottom line.

---

## Priority 10: Instructions Without Purpose

**Practice:** Add context to improve performance

**Signals:**
- Formatting rules without explanation ("always bold the first word")
- Constraints with no rationale ("keep it under 100 words")
- Style requirements with no audience context ("be formal")
- "Because I said so" energy — rules that look arbitrary without the why
- The prompt is primarily a list of formatting/style constraints with no stated task or rationale

**Skip if:** The purpose is self-evident (e.g., "keep it under 280 characters" — obviously for a tweet).

---

## Priority 11: Sequential Independent Operations

**Practice:** Use parallel tool calling

**Signals:**
- "Read file A, then read file B, then read file C"
- Multiple API calls or searches described in sequence when they don't depend on each other
- "First check X, then check Y, then check Z" where the checks are independent

**Skip if:** The operations genuinely depend on each other (step 2 uses output of step 1).

---

## Priority 12: Questions About Unseen Content

**Practice:** Minimize hallucinations

**Signals:**
- "What does [specific file/function/line] do?" without providing the content
- Questions about specific data that hasn't been loaded or read
- "Is there a bug in my code?" without the code being present in context
- Assumptions about file structure or content without verification

**Skip if:** The content is already in the conversation context or the user just shared it.

---

## When to Skip Coaching Entirely

Don't add a coaching nudge when:

- The prompt is a simple factual question ("What year was Python created?")
- It's a one-word or very short command ("yes," "continue," "next")
- The user is responding to a follow-up question you asked
- The prompt is already well-structured with multiple best practices applied
- You've already coached on this specific practice earlier in the conversation
- The user has asked you to stop coaching or indicated they don't want tips
