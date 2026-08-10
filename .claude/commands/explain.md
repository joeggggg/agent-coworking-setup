---
description: Produce a structured, comprehensive explanation of a topic using the mandated learning framework — overview, main content, and knowledge reinforcement.
---

When the user invokes /explain, produce a response that strictly follows this structure. Do not skip any section.

```markdown
# [Topic Name]

## Quick Overview
- **Learning Objectives**: 2–3 specific things the user will be able to do after reading
- **Prerequisites**: concepts the user should already know
- **Key Takeaways**: 3–4 bullet points summarizing the essentials

## Main Content (max 7 subtopics)

### [Subtopic 1]
Clear explanation using **bold** for key terms, *italics* for definitions.
Include an analogy or example grounded in the user's domain where relevant — see `[CUSTOMIZE: context bias]` in `.claude/CLAUDE.md`.

### [Subtopic 2]
Continue the logical breakdown with visual markers:
- 📌 key point
- 💡 insight
- ℹ️ additional info
- 🔗 related concept
- ✅ best practice
- 📖 further elaboration question

[Repeat for each subtopic — maximum 7]

## Knowledge Reinforcement

- **Summary Points**: reiterate the most critical points
- **Key Terms & Glossary**: new concepts, theories, terms in a table

  | Term | Definition | Related To |
  |------|------------|------------|

- **Practice Questions**: 2–3 targeted questions to check understanding
- **Practical Exercise**: a small, relevant exercise tied to the user's domain
- **Related Concepts**: links to other areas worth exploring next
```

## Behavioural rules for /explain
- MECE structure — each subtopic must be non-overlapping and collectively exhaustive
- Ground analogies in the user's stated domain (`[CUSTOMIZE]` in `.claude/CLAUDE.md`) rather than generic examples
- If the topic is broad (5+ major subtopics), recommend splitting it and ask which area to start with
- Embed at least one Mermaid diagram if the topic involves a process, hierarchy, or sequence
