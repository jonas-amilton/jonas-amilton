# Base Methodology

This reference summarizes the operating methodology for sustaining engineering.

## Smallest safe solution

The core rule is to reduce scope and code without reducing understanding or safety.

Practical application:

- read the real flow before choosing a short solution;
- reuse existing code before creating a new helper;
- prefer stdlib, platform features, and installed dependencies before adding dependencies;
- delete code introduced by your own change when it becomes unnecessary;
- do not cut validation, security, accessibility, data-loss handling, or essential tests.

Anti-patterns:

- creating an abstraction “for the future”;
- installing a library for a native feature;
- explaining at length a solution that should be simple;
- fixing only the reported path when the cause is in shared code.

## Verifiable process

The core rule is to turn engineering into cycles with clear gates.

Practical application:

- debugging follows phases: root cause, pattern analysis, hypothesis/test, implementation;
- pragmatic TDD for bugfixes: failing reproduction before the fix, then green;
- verify before conclusion: run a fresh command, read output, then claim status;
- if multiple attempts fail, stop and reassess the hypothesis or architecture.

Anti-patterns:

- “quick fix” before understanding;
- multiple changes before measuring which one worked;
- a test that passes immediately and never proved the bug;
- claiming “fixed” because the diff looks right.

## Coding discipline: caution and surgical changes

The core rule is to avoid assumptions and avoid silently expanding scope.

Practical application:

- state assumptions and ambiguities;
- do only what traces to the request;
- keep the existing style;
- remove only mess created by your own change;
- define verifiable success before implementation.

Anti-patterns:

- choosing one ambiguous interpretation without saying so;
- refactoring adjacent code because “it was there”;
- hiding a trade-off or uncertainty;
- changing many files without a traceable need.

## Brevity without technical loss

The core rule is to say less without thinking less. Remove filler, not precision.

Practical application:

- keep code, commands, errors, paths, numbers, and payloads byte-for-byte exact;
- reduce redundant explanation;
- prefer short, actionable bullets;
- preserve the minimum context needed for decision, test, and rollback;
- in production work, do not sacrifice evidence, risk, or trade-off for concision.

Anti-patterns:

- long responses that repeat the obvious;
- summaries so short they omit cause, validation, or risk;
- changing the user's language without asking;
- compressing a command, stack trace, or error message imprecisely.

## Sustaining synthesis

1. **Understand deeply**: flow, callers, environment, and evidence.
2. **Reduce aggressively**: smallest correction at the right point.
3. **Protect production**: timeout, idempotency, observability, and rollback when applicable.
4. **Say little, prove much**: short answer, fresh test/reproduction/check.
