# prompt-coach

Add one relevant prompting improvement after substantive Claude requests.

![prompt-coach preview](prompt-coach-preview.png)

## Who it is for

prompt-coach is for people who work with Claude often enough that prompt quality affects their output, but who do not want a lecture or a separate prompting workflow.

It is part of grAIce Tech's work on practical AI systems, agentic workflows, and operational clarity.

## Problem it solves

Most prompting advice is either too broad to apply in the moment or too intrusive to use while work is moving. Teams need small, contextual feedback that improves the next request without interrupting the current one.

prompt-coach completes the user's request first, then adds one short prompting nudge only when it is useful.

## Quickstart

```bash
cp -r prompt-coach ~/.claude/skills/
```

## Example output

User prompt:

> Categorize these support tickets as urgent, normal, or low.

Claude completes the categorization, then may add:

```text
---
Prompt tip - Use Examples Effectively
You could include 3-5 example categorizations so Claude knows where you
draw the line between "urgent" and "normal."
```

## How it works

The skill checks substantive prompts against 12 prompting practices from Anthropic's official prompting guidance.

It can detect issues such as:

- Missing examples for category or style-sensitive work
- Vague instructions with unclear format, audience, or length
- Missing success criteria
- Missing context about why the task matters
- Complex requests that would benefit from structure or chaining

The behavior is defined in `SKILL.md`. Supporting references live in `references/`, and evaluation cases live in `evals/`.

## Status / roadmap

Status: usable Claude skill with 12 detection patterns and eval coverage.

Planned cleanup:

- Refine examples as prompting guidance evolves.
- Keep nudges brief and non-intrusive.
- Add more evals for false positives and cases where the skill should stay quiet.

## License

MIT. Built by [grAIce Tech](https://github.com/grAIcetech).

Best practices are sourced from [Anthropic's official prompting guide](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/claude-prompting-best-practices).
