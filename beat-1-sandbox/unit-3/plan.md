# Plan: Fix ArgumentError in _check_skip() and IngestedSource Model to Prevent Duplicate Ingestion

## Issue
- Repo: codepath/pathreview-ai301-fa26-s1
- Issue: #1 (https://github.com/codepath/pathreview-ai301-fa26-s1/issues/1)

## Diagnosis
In `ingestion/pipeline.py`, `_check_skip()` attempts to check whether content was previously ingested by querying `self.db_session.query("IngestedSource").filter_by(source_id=source_id).first()`.

This fails in three coupled ways:
1. `query("IngestedSource")` passes a string literal instead of the declarative class, raising `sqlalchemy.exc.ArgumentError: Textual column expression 'IngestedSource' should be explicitly declared with text('IngestedSource'), or use column('IngestedSource') for more specificity`.
2. The `IngestedSource` model (`core/models/ingested_source.py`) lacks the `source_id` column targeted by `filter_by(source_id=source_id)`. Passing the mapped class without adding `source_id` raises an `InvalidRequestError` or `AttributeError`, which the broad `except Exception` block swallows before returning `None`. While `content_hash` exists, callers throughout the ingestion pipeline construct composite identifiers (`f"{type}_{profile_id}_{hash}"`) that represent the exact logical document instance and query by `source_id`.
3. Additionally, `_record_ingested_source()` currently only logs without persisting an `IngestedSource` record to the database session, preventing any subsequent deduplication lookup from matching.

Quoting Repro Evidence:
`WARNING [ingestion.pipeline] Could not check if source already ingested source_id=test-repo-123 error=Textual column expression 'IngestedSource' should be explicitly declared with text('IngestedSource')...`

Consequently, `_check_skip()` consistently returns `None`, leading to duplicate embedding generation during re-ingestion.

## Scope
- In-Scope:
  - Add `source_id: Mapped[str | None] = mapped_column(String(255), nullable=True, index=True)` to `IngestedSource` in `core/models/ingested_source.py`.
  - Add Alembic migration `003_add_source_id_to_ingested_sources.py` with `down_revision = "002_add_error_message_to_reviews"`.
  - Import `IngestedSource` in `ingestion/pipeline.py` and update `_check_skip()` to use `self.db_session.query(IngestedSource).filter_by(source_id=source_id).first()`.
  - Update `_record_ingested_source()` to persist `IngestedSource` to `self.db_session` when active (`self.db_session.add(record)` and `self.db_session.commit()`).
  - Add unit test marked with `@pytest.mark.unit` in `tests/unit/test_ingestion_pipeline.py`.
- Not-in-Scope:
  - Changes to embedding models, tokenizers, or chunking algorithms.
  - Modifications to API endpoints or foreign key relations.

## Files Touched
- `core/models/ingested_source.py`
- `alembic/versions/003_add_source_id_to_ingested_sources.py`
- `ingestion/pipeline.py`
- `tests/unit/test_ingestion_pipeline.py`

## Implementation Approach
1. Update `core/models/ingested_source.py`:
   - Declare `source_id: Mapped[str | None] = mapped_column(String(255), nullable=True, index=True)` on `IngestedSource`.
   - Add index `Index("ix_ingested_sources_source_id", "source_id")` to `__table_args__`.
2. Add Alembic migration `alembic/versions/003_add_source_id_to_ingested_sources.py`:
   - Set revision chain: `down_revision = "002_add_error_message_to_reviews"`.
   - In `upgrade()`, add column `source_id` (String(255)) and create index on `ingested_sources`.
3. Update `ingestion/pipeline.py`:
   - Import `IngestedSource` from `core.models.ingested_source`.
   - Update `_check_skip()` to execute `self.db_session.query(IngestedSource).filter_by(source_id=source_id).first()`.
   - In `_record_ingested_source()`, if `self.db_session` is present, instantiate `IngestedSource(source_id=source_id, source_type=source_type, profile_id=profile_id, chunk_count=chunk_count)`, call `self.db_session.add(...)` and `self.db_session.commit()`.
4. Add regression tests with `@pytest.mark.unit` in `tests/unit/test_ingestion_pipeline.py`:
   - Test that calling `_check_skip` against an existing seeded `IngestedSource` returns an `IngestResult` with `skipped=True` and `skip_reason="Source already ingested"`.
   - Test that subsequent calls to `ingest_resume` skip duplicate processing when the source has been recorded.

## Test Plan
- Run unit test suite:
  `pytest tests/unit/test_ingestion_pipeline.py -v`
- Run full unit suite:
  `make test-unit`
- Reproduction verification:
  - Seed an `IngestedSource` row with `source_id="test-repo-123"`.
  - Invoke `_check_skip("test-repo-123", "repo")`.
  - Verify result is `IngestResult(source_id="test-repo-123", skipped=True, ...)`.

## Risks and Unknowns
- Test SQLite isolation: Use mocked session or fixture-provided `Profile` so foreign keys in unit tests validate cleanly.

## Deviations
None. Plan is ready for review and implementation.
