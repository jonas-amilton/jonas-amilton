# Resolved-Problem Memory

Use this reference to create a lightweight, versionable, searchable memory of resolved problems. The memory should support search, handoff, capture, consolidation, recall, and continuous improvement from completed troubleshooting work.

## Goal

Avoid reinvestigating the same problem and help the skill improve from validated experience. Each entry should answer quickly:

- have we seen this symptom before?
- what validated root cause did we find?
- what was the smallest safe patch?
- which files/lines and breakpoints helped?
- which test proved the fix?
- which signal indicates recurrence?
- should this learning become memory, a skill rule, a runbook, an alert, or a test?

## Where to store it

Preference order:

1. If the repository already has a knowledge/runbook area, use it.
2. If there is no convention, propose `.sustaining/memory/resolved-problems/<slug>.md` in the user's project.
3. If the user does not want a new file, provide the entry as markdown to paste into a wiki/ticket.

Never store secrets, tokens, sensitive payloads, unnecessary PII, or customer data. Redact sensitive values and preserve only format, type, and technical evidence.

## Learning loop

Run this for every relevant problem:

1. **Recall**: search memories, tickets, runbooks, commits, and similar tests before investigating from scratch.
2. **Resolve**: apply the root-cause, minimal-patch, breakpoint, and fresh-verification workflow.
3. **Distill**: after resolving, summarize the lesson into a short operational rule: when to apply it, required evidence, typical patch, and test.
4. **Promote**: decide the learning destination:
   - episodic memory: a specific case that can recur;
   - skill update: a general rule that changes future behavior;
   - runbook: repeatable operational sequence;
   - test/alert: automatable prevention;
   - discard: one-off case, unlikely reuse, or insufficient evidence.
5. **Review**: periodically remove duplicates, obsolete entries, and contradictions.

Do not promote everything automatically. Learning enters a skill/runbook only when it is reusable, safe, verifiable, and not too specific to one isolated incident.

## Consult memory before investigating

Search for specific terms, not generic terms:

```bash
rg -n "<exact-error>|<stack-frame>|<endpoint>|<job>|<table>|<feature-flag>" .sustaining docs runbooks . 2>/dev/null
```

When using an old entry, classify it:

- **Reused**: symptoms, component, and cause match; still validate against current code.
- **Similar**: helps form a hypothesis, but is not proof.
- **Discarded**: looked similar, but evidence diverged; explain why.

## Record after resolving

Create or propose an entry only when evidence is sufficient. Without a validated root cause, record it as a discarded hypothesis or inconclusive investigation, not as a resolved problem.

Template:

```markdown
# <short problem slug>

## Summary
- Date: YYYY-MM-DD
- System/component: <name>
- Severity: low | medium | high | incident
- Status: resolved | mitigated | recurrence monitored

## Symptoms
- Exact error/log/metric: `<message>`
- Endpoint/job/screen: `<location>`
- Frequency/impact: <evidence>

## Validated root cause
Fact: <observed evidence>.
Validated hypothesis: <causal mechanism>.
How it was validated: <test, log, metric, tracing, reproduction>.

## Useful files and breakpoints
- `path/to/file.ext:line`: role in the flow; evidence.
- Breakpoint `path/to/file.ext:line`: condition; inspect `<variable>`; validates `<hypothesis>`.

## Minimal patch applied
- Changed files: `<paths>`
- Strategy: validation | shared rule | timeout/retry | idempotency | config | rollback
- Why it was the smallest safe patch: <justification>

## Verification
- Test/command: `<exact command>`
- Expected result: <result>
- Observed result: <result>

## Rollback and residual risk
- Rollback: <how to revert>
- Residual risk: <what can still fail>
- Recurrence signal: <log/metric/alert>

## Learning distillation
- Reusable rule: <when to apply again>
- Minimum evidence before applying: <logs/tests/paths>
- Recommended destination: memory | update skill | runbook | test | alert | discard
- Reason: <likely reuse and risk>

## Tags
`#component` `#error` `#cause` `#patch-type`
```

## Consolidate memory

Periodically review old entries:

- merge duplicates that describe the same causal mechanism;
- mark entries obsolete when paths, configs, or contracts change;
- promote recurring rules to a runbook, automated test, or skill update;
- keep entries short, but preserve commands, paths, lines, and evidence.

## Quality rules

- Memory does not replace current validation: always revalidate against the current code, environment, and version.
- An entry must contain fact, cause, patch, test, rollback, and promotion decision; otherwise it is an investigation note.
- Prefer simple markdown searchable with `rg` over complex formats.
- Use stable slugs: `yyyy-mm-dd-component-symptom-cause.md` when useful.
- Do not record content that cannot be shared with the team.
