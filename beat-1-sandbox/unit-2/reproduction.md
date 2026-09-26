# Unit 2 — Claim and Reproduce

Path: `beat-1-sandbox/unit-2/reproduction.md`

Record of your claim and reproduction on the issue you chose in Unit 1, and of the
evaluation runs that produced `eval-run.txt`. This file is graded at the path above; a copy
kept anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your identity upstream

**GitHub username**

VenkataSriSaiSuryaMandava

---

## Posted upstream

**Claim comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/1#issuecomment-5848964655
Hi maintainers,
I would like to claim this issue as part of AI301 (Fall 2026, Section 1).
Plan of investigation:
Set up the local Python/database environment on macOS and establish a clean baseline.
Reproduce the duplicate embeddings behavior during re-ingestion by tracing _check_skip() and its query parameter passing to db_session.query().
Verify the resulting database state and post the minimal reproduction steps with terminal logs back to this thread.

**Reproduction comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/1#issuecomment-5849142890

### Environment
* **Platform:** macOS Sonoma (Apple Silicon arm64)
* **Python Runtime:** Python 3.14.1 / SQLLhint 2.1.1
*`*Commit:** `f89c06fc3ff292df2a04a39ac51319d32a76b779` (HEAD of `codepath/pathreview-ai301-fa26-s1`)

### Steps to Reproduce
1. In a clean virtual environment, install dependencies:
   ```bash
   pip install -e ".[dev]"
   ```
2. Inspect `ingestion/pipeline.py:288-318` inside `_check_skip()`:
   ```python
   existing = (
       self.db_session.query("IngestedSource")
       .filter_by(source_id=source_id)
       .first()
   )
   ```
3. Seed an existing record in `IngestedSource` with `source_id="test-repo-123"`.
4. Trigger `_check_skip(source_id="test-repo-123", source_type="repo")` during re-ingestion.

### Observed Behavior
`self.db_session.query("IngestedSource")` passes a string literal instead of the declarative class, raising `sqlalchemy.exc.ArgumentError`:
```text
ArgumentError: Textual column expression 'IngestedSource' should be explicitly declared with text('IngestedSource'), or use column('IngestedSource') for more specificity
```
The broad `except Exception as e:` in `_check_skip()` catches this error, logs a warning, and returns `None`:
```text
WARNING [ingestion.pipeline] Could not check if source already ingested source_id=test-repo-123 error=Textual column expression 'IngestedSource' should be explicitly declared with text('IngestedSource')...
```
Because `_check_skip()` returns `None`, the caller proceeds with re-parsing, re-chunking, and re-generating duplicate vector embeddings for an already-ingested source.

### Expected Behavior
`_check_skip()` should query using the declarative mapped model `IngestedSource` from `core.models.ingested_source`:
```python
existing = (
    self.db_session.query(IngestedSource)
    .filter_by(source_id=source_id)
    .first()
)
```
When queried with the model class, `existing` evaluates to the stored record, and `_check_skip()` returns:
```python
IngestResult(
    source_id=source_id,
    chunk_count=0,
    skipped=True,
    skip_reason="Source already ingested",
)
```

> Per repro-check conventions, this report isolates the exact breaking query trace without precommitting to fixes or delivery dates.

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. 17/20 scored items (categories: clear-accept 5/8, disclosure 1/1, no-evidence 4/4, unfollowable-comms 3/3, wrong-target 4/4)
2. 19/20 scored items (categories: clear-accept 7/8, disclosure 1/1, no-evidence 4/4, unfollowable-comms 3/3, wrong-target 4/4)

**Package analysis**

Package `pkg-10`:
Gold label: accept (`honest cannot-reproduce: exact layout and config, prompt artifact shown, names the environment differences (Linux+zsh vs macOS+fish) and the PWD-resolution hypothesis for why fish matters`).
Rubric verdict:
- Initial run: reject (failed on `behavior-matches` because the initial condition strictly required the artifact to demonstrate the reported bug symptom, which an un-reproduced run cannot display).
- Revised run: accept (after updating `behavior-matches` and `honest-reporting` pass conditions to explicitly accept evidenced cannot-reproduce packages when accompanied by command execution artifacts and documented divergence hypotheses).

**Check rationale**

"Pass if the output demonstrates the reported symptom, OR if an honest cannot-reproduce report displays artifacts confirming the exact commands executed alongside the observed non-triggering output; fail if no artifact is provided or the artifact shows an unrelated failure condition"

This check was updated from requiring raw failure traces to accommodating honest, evidenced non-reproductions. The initial formulation rejected valid investigations such as `pkg-09` and `pkg-10` where contributors faithfully executed reproduction steps, included real terminal artifacts, and identified environment divergence.

**Trade-offs**

Broadening `behavior-matches` to accept negative reproduction artifacts introduces the risk of false-positive accepts if a contributor runs arbitrary commands and claims non-reproduction. We mitigated this by requiring the artifact to confirm the exact execution commands from the issue, verified by running `--only pkg-01,pkg-09,pkg-10,pkg-12` where `pkg-01` served as a canary to confirm that standard positive reproductions remained unaffected.

---
"Related paths: `eval-run.txt` in this directory; your skill's files in `tools/repro-check/`.
