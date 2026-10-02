# Unit 3 — Plan and Build

Path: `beat-1-sandbox/unit-3/plan-and-implement.md`

Record of your plan, the branch you built it on, and the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in the
repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Posted upstream

**GitHub username**

VenkataSriSaiSuryaMandava

**Plan comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/1#issuecomment-5946754853

### Diagnosis
In `ingestion/pipeline.py`, `_check_skip()` queries `self.db_session.query("IngestedSource").filter_by(source_id=source_id)`.
As captured in reproduction report https://github.com/codepath/pathreview-ai301-fa26-s1/issues/1#issuecomment-5849142890, this raises:
`ArgumentError: Textual column expression 'IngestedSource' should be explicitly declared with text('IngestedSource'), or use column('IngestedSource') for more specificity`

The `except Exception` block swallows this error and returns `None`. Additionally, `IngestedSource` in `core/models/ingested_source.py` currently lacks the `source_id` column used by pipeline queries, and `_record_ingested_source()` only logs rather than persisting records to `self.db_session`.

### Proposed Changes
- `core/models/ingested_source.py` & Alembic migration: Add indexed `source_id` column to `IngestedSource`.
- `ingestion/pipeline.py`: Import `IngestedSource` and update `_check_skip()` to query `self.db_session.query(IngestedSource).filter_by(source_id=source_id).first()`. Update `_record_ingested_source()` to persist `IngestedSource` records with `db_session.commit()`.
- `tests/unit/test_ingestion_pipeline.py`: Add unit tests validating skip detection and deduplication with `@pytest.mark.unit`.

### Verification
- Seed an `IngestedSource` row with `source_id="test-repo-123"` and invoke `_check_skip("test-repo-123", "repo")`: verify return value is `IngestResult(skipped=True)` with no `ArgumentError`.
- Execute `make test-unit` to confirm all unit tests pass cleanly.

---

## Your branch

**Branch**

fix/1-check-skip-query

**Evidence**

### Before
Command:
```bash
python3 -c "
import logging
from unittest.mock import MagicMock
from sqlalchemy import create_engine
from sqlalchemy.orm import Session
from ingestion.pipeline import IngestionPipeline

logging.basicConfig(level=logging.WARNING)

engine = create_engine('sqlite:///:memory:')
with Session(engine) as session:
    pipeline = IngestionPipeline(
        db_session=session,
        vector_db=MagicMock(),
        embedding_provider=MagicMock()
    )
    result = pipeline._check_skip(source_id='test-repo-123', source_type='repo')
    print('BEFORE RESULT:', result)
"
```

Output:
```
2026-09-29 05:49:59 [warning ] Could not check if source already ingested error="Textual column expression 'IngestedSource' should be explicitly declared with text('IngestedSource'), or use column('IngestedSource') for more specificity" source_id=test-repo-123
BEFORE RESULT: None
```

### After

Command:

```bash
.venv/bin/python3 -c "
import logging
from unittest.mock import MagicMock
from uuid import uuid4
from sqlalchemy import create_engine
from sqlalchemy.orm import Session
from core.models.ingested_source import IngestedSource
from ingestion.pipeline import IngestionPipeline

logging.basicConfig(level=logging.INFO)

engine = create_engine('sqlite:///:memory:')
IngestedSource.__table__.create(engine, checkfirst=True)

with Session(engine) as session:
    pipeline = IngestionPipeline(
        db_session=session,
        vector_db=MagicMock(),
        embedding_provider=MagicMock()
    )

    # 1. First run records source
    pipeline._record_ingested_source('test-repo-123', 'repo', str(uuid4()), 5)

    # 2. Re-ingestion check
    result = pipeline._check_skip(source_id='test-repo-123', source_type='repo')
    print('REPRO RESULT:', result)
"
```

Output:
```
2026-10-02 16:40:03 [info      ] Recording ingested source      chunk_count=5 profile_id=73260b02-90e2-475b-a6a6-9f179643d8ba source_id=test-repo-123 source_type=repo
2026-10-02 16:40:03 [info      ] Source already ingested, skipping source_id=test-repo-123
REPRO RESULT: IngestResult(source_id='test-repo-123', chunk_count=0, skipped=True, skip_reason='Source already ingested')
```

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these fields.

**Run history**

20/20 scored items

**Package analysis**

`pkg-04`: Rubric decided `reject`, gold label was `reject`. In `pkg-04`, the plan proposes a workaround that bypasses maintainer instructions specified directly in the issue thread, and ignores explicit repository conventions regarding disclosure and scope. The rubric evaluates `thread-convention` against `Thread highlights` and `Repo facts`: because the proposed plan fails to adhere to maintainer direction in the thread, `thread-convention` receives a grade of `fail`. Under the verdict rule, any required check failing results in a package decision of `reject`, matching the gold label.

**Check rationale**

"| grounded-diagnosis | Plan's stated diagnosis/root cause read against `Repro evidence` (and `Issue` description) | Pass if the identified root cause and error mechanism directly explain and align with the failure symptoms, logs, or error traces shown in the reproduction evidence; fail if the diagnosis contradicts, ignores, or misattributes the proven breakdown to an unaffected component or layer. | required |"

This check was refined from an earlier, looser definition that only checked whether the plan had a non-empty Diagnosis section. We revised it to strictly evaluate alignment against literal reproduction logs and error traces (`Repro evidence`) because plans that misattributed failures to unrelated parts of the codebase were previously slipping through as plausible-sounding but completely incorrect.

**Trade-offs**

By requiring `grounded-diagnosis` to strictly match the symptoms and logs in `Repro evidence`, the check accepts that it will reject plans where the author has identified a real upstream root cause that differs superficially from the initial symptom trace, until the author provides an explicit reproduction trace connecting the two. Nothing changed elsewhere in the benchmark suites across the remaining categories, and we know this because the eval run achieved 20/20 agreement (PASS) with 0 regressions across all 5 categories (`clear-accept 7/7`, `scope-creep 4/4`, `thread-convention 2/2`, `unbuildable 3/3`, `wrong-cause 4/4`).

Related paths: `plan.md` and `eval-run.txt` in this directory; your skill's files in `tools/plan-check/`.
