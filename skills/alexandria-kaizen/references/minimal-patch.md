# Minimal Patch, Files, and Breakpoints

Use this reference when the user asks for bug diagnosis, fix plan, breakpoint, minimal patch, or debug script.

## Related file map

Required format when a codebase is available:

```markdown
### Related files
- `path/to/entrypoint.ext:10`: flow entry; receives payload X.
- `path/to/service.ext:42`: likely business rule; transforms X into Y.
- `path/to/repository.ext:87`: persistent I/O; where data fails/duplicates/is lost.
- `path/to/test.ext:15`: existing test or recommended regression-test location.
```

Rules:

- Use paths relative to the repository root.
- Cite exact line when you have local evidence.
- If the line comes from inference, write `estimated line` and state the command to confirm it.
- Include discarded files only when it reduces ambiguity: “read, but out of flow because...”.

## Suggested breakpoints

Required format when investigation needs interactive debugging:

```markdown
### Suggested breakpoints
- `src/api/orders.ts:31`
  - Condition: `orderId == "<affected id>"`
  - Inspect: `payload`, `userId`, `idempotencyKey`
  - Validates: input arrives correctly before validation.
- `src/domain/pricing.ts:88`
  - Condition: `discount != null`
  - Inspect: `basePrice`, `discount`, `finalPrice`
  - Validates: calculation or rounding is the cause.
```

Rules:

- Prefer breakpoints at boundaries: input, validation, transformation, I/O, output.
- Each breakpoint must test one specific hypothesis.
- Do not suggest generic breakpoints on every method.
- For concurrency, prefer conditional logpoints and metrics; traditional breakpoints can mask race conditions.

## Minimal-patch boilerplates

### Bugfix with input validation

```markdown
### Minimal patch
Fact: `file:line` receives invalid value without normalization.
Hypothesis: normalizing/validating at the boundary prevents failure for all callers.
Change:
- `boundary-file.ext`: add validation/normalization before calling domain code.
Test:
- `test-file.ext`: invalid-payload/regression case.
Rollback:
- Revert boundary-file change; no data migration.
```

### Bugfix in shared rule

```markdown
### Minimal patch
Fact: multiple callers pass through helper `X` in `file:line`.
Hypothesis: fixing `X` resolves all paths without duplicated patches.
Change:
- `helper-file.ext`: adjust the minimum condition while preserving the existing contract.
Test:
- helper unit test; one integration test for the reported path if available.
Rollback:
- Revert helper; feature flag if present.
```

### External I/O with timeout/retry

```markdown
### Minimal patch
Fact: external call in `file:line` has no safe timeout/retry.
Hypothesis: timeout + bounded retry for an idempotent operation reduces transient failure without duplicating effects.
Change:
- add explicit timeout;
- retry with limit/backoff only when idempotency or deduplication exists;
- structured log with correlation id, without PII/secrets.
Test:
- simulate timeout and transient error; validate attempts and fallback.
Rollback:
- Revert client/config; keep alert for error rate.
```

### Operational script

```markdown
### Minimal patch
Fact: data needs correction/reprocessing.
Hypothesis: idempotent batched script fixes it without duplicating effects.
Change:
- script with `--dry-run` default;
- small batch, checkpoint, and counter summary;
- timeout/bounded retry for I/O;
- logs without PII/secrets.
Test:
- dry-run on sample; fixture run; rerun does not change counters incorrectly.
Rollback:
- backup/snapshot or reverse script validated on sample.
```
