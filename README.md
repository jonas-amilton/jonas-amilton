# Alexandria Kaizen

Repository for the `alexandria-kaizen` skill: a production-support workflow for incident response, bugfixes, minimal patches, and reusable learning from resolved problems. The name combines Alexandria as a knowledge-library metaphor with Kaizen as the continuous-improvement loop used by the skill.

## What this skill does

Use it when you need to:

- investigate production incidents or regressions;
- find related files and candidate line numbers;
- propose useful breakpoint or logpoint locations;
- design the smallest safe patch;
- preserve evidence, rollback path, and operational safety;
- record resolved-problem memory so future investigations start with prior knowledge.

## Repository layout

```text
skills/alexandria-kaizen/
├── SKILL.md
├── agents/
│   └── openai.yaml
└── references/
    ├── base-methodology.md
    ├── incident-response.md
    ├── minimal-patch.md
    └── resolved-problem-memory.md
```

## Installation

Clone the repository once:

```bash
git clone https://github.com/jonas-amilton/alexandria-kaizen.git
cd alexandria-kaizen
```

### Claude Code

Claude Code discovers personal skills from `~/.claude/skills/<skill-name>/SKILL.md` and project skills from `.claude/skills/<skill-name>/SKILL.md`.

Personal install:

```bash
mkdir -p ~/.claude/skills
cp -R skills/alexandria-kaizen ~/.claude/skills/
```

Project-local install:

```bash
mkdir -p .claude/skills
cp -R skills/alexandria-kaizen .claude/skills/
```

Invoke directly in Claude Code with:

```text
/alexandria-kaizen
```

### OpenCode

OpenCode discovers skills from `.opencode/skills`, `~/.config/opencode/skills`, `.claude/skills`, `~/.claude/skills`, `.agents/skills`, and `~/.agents/skills`.

Project-local install:

```bash
mkdir -p .opencode/skills
cp -R skills/alexandria-kaizen .opencode/skills/
```

Global install:

```bash
mkdir -p ~/.config/opencode/skills
cp -R skills/alexandria-kaizen ~/.config/opencode/skills/
```

OpenCode loads available skills through its native skill tool. If the skill is not visible, verify that the folder name matches `name: alexandria-kaizen` and that `SKILL.md` is uppercase.

### Codex

Codex reads skills from repository and user `.agents/skills` locations. Use a repository install when the skill should travel with the project, or a user install when it should apply across repositories.

Repository install:

```bash
mkdir -p .agents/skills
cp -R skills/alexandria-kaizen .agents/skills/
```

User install:

```bash
mkdir -p ~/.agents/skills
cp -R skills/alexandria-kaizen ~/.agents/skills/
```

Invoke explicitly in Codex with:

```text
$alexandria-kaizen
```

Restart the tool if the skill does not appear after copying it.

## Invocation

Invoke the skill explicitly by name:

```text
Use $alexandria-kaizen to investigate this production bug with root cause, mitigation, minimal patch, and verification.
```

## Expected output

For troubleshooting, the skill should produce sections like:

```markdown
### Context
### Observed symptoms
### Memory consulted
### Related files
### Suggested breakpoints
### Facts
### Hypotheses
### Validation performed
### Root cause
### Immediate mitigation
### Minimal patch
### Proposed memory
### Learning / promote to skill or runbook
### Definitive fix
### Prevention / observability
### Tests
```

## Usage workflow

1. **Frame the problem**: capture context, symptoms, impact, and success criteria.
2. **Recall prior knowledge**: search resolved-problem memory and existing runbooks.
3. **Investigate with evidence**: trace the flow, identify related files, and define breakpoint/logpoint candidates.
4. **Patch minimally**: choose the smallest safe change at the shared point.
5. **Verify freshly**: run commands/tests now and read the output.
6. **Capture learning**: propose memory entries only when cause, patch, rollback, and validation are known.

## Safety rules

- Do not store secrets, tokens, sensitive payloads, unnecessary PII, or customer data in memory entries.
- Mark inferred file lines as estimated and include the command needed to confirm them.
- Prefer reversible mitigation before large code changes during active incidents.
- Add timeout, bounded retry, fallback, idempotency, and observability only when the real risk requires them.
- Do not use emojis in responses, templates, memory entries, commit messages, or generated documentation.
- Do not promote every incident into a rule; promote only reusable, verified, operationally actionable learning.

## Maintaining the skill

When changing the skill:

1. Keep `SKILL.md` concise; move detailed workflows into `references/`.
2. Keep references one level below `SKILL.md` and link them from the main file.
3. Validate that `agents/openai.yaml` still uses the correct skill name.
4. Search for accidental source-specific references before committing.
5. Use commit messages that describe the skill change without mentioning model families or vendor-specific model names.
