## Context

This is a blog post for fuzzwah.com covering a real upgrade experience: migrating a Python monorepo from OpenSpec's old command-based workflow system to the skills-based system introduced in 1.2.0. The post documents the migration mechanics, explains the architectural distinction between Claude Code commands and skills, and makes the broader argument for spec-driven development and treating `.claude/` as first-class project infrastructure.

The blog uses Jekyll with a consistent post format. All posts are written in the author's established voice: conversational, warm, Australian, practical, and opinionated. The audience is developers building real projects with AI coding tools.

## Goals / Non-Goals

**Goals:**
- Document the specific migration path (commands removed, skills created, manual cleanup required)
- Explain the commands vs skills distinction in Claude Code in concrete, practical terms
- Make a genuine case for spec-driven development overhead — honest about the cost, honest about the value
- Illustrate the `openspec update` experience as a model for how AI tooling upgrades should work
- Advance the `.claude/` as infrastructure argument

**Non-Goals:**
- Not a general OpenSpec tutorial or getting-started guide
- Not a technical deep-dive into Claude Code internals
- Not a comparison of OpenSpec against other tools

## Decisions

**Lead with the migration experience, not the concept**
The hook is the moment `openspec update --force` ran and cleanly handled 7 file operations. Starting with the concrete experience before the abstract concept keeps the post grounded. This audience distrusts concept-first writing.

**Treat commands vs skills as the conceptual centrepiece**
This is the architectural insight the post turns on. Commands are one-shot; skills are persistent context. The rest of the post radiates outward from this distinction.

**Use the spec-driven overhead argument as a genuine moment of honesty**
Rather than boosterism, acknowledge the cycle is heavy. Then make the case: the friction is load-bearing. It stops the agent from drifting. This framing will land better with experienced practitioners than enthusiasm would.

**End on `.claude/` as infrastructure**
The closing argument — your `.claude/` directory is project infrastructure, not config — is the most transferable idea in the post. It applies beyond OpenSpec and gives readers something actionable.

## Risks / Trade-offs

- **Risk**: Post becomes too abstract in the commands vs skills section → Mitigation: anchor every conceptual claim to a specific example from the migration
- **Risk**: Spec-driven overhead section reads as defensive → Mitigation: lead with "yes it's heavy" before making the case; don't try to minimise the cost
- **Risk**: Post length creeps beyond ~1,400 words → Mitigation: cut from the migration mechanics section first; the conceptual and argument sections are the payload
