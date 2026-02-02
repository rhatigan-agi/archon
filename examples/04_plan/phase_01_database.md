# Phase 1 — Verify ProjectTask Model Relationships

**Intent**: INTENT_007  
**Phase Type**: database  
**Depends On**: None

---

## Grounding (Existing State)

**Existing Models and Fields** (from Discovery Pass 2 — AUTHORITATIVE):

- `ProjectTask` model exists in `apps/api/models/project.py` with fields:
  - `id` (UUIDField)
  - `project` (ForeignKey → Project)
  - `title` (CharField)
  - `description` (TextField)
  - `status` (CharField, choices: Open, In-Progress, Closed)
  - `assigned_to` (ForeignKey → User)
  - `due_at` (DateTimeField, nullable)
  - `completed_at` (DateTimeField, nullable)
  - `created_at` (DateTimeField)

- `Project` model exists with reverse relationship to `ProjectTask` via `project` ForeignKey

**Schema Sufficiency** (from Discovery Pass 4 — DETERMINISTIC):

```
ProjectTask.migration_required: false
Reason: All required fields exist
```

**CRITICAL**: No new model fields are needed. All required fields already exist per Discovery.

---

## Acceptance Criteria Addressed

- [x] Verify that `ProjectTask` model relationships (to `Project` and `User`) are correctly utilized
- [x] Confirm existing `ProjectTask` model supports status field (Open/In-Progress/Closed)
- [x] Confirm `assigned_to` field is available for task assignment

---

## Files to Modify

- `apps/api/models/project.py` — **VERIFY ONLY, no changes needed**

## Files That May Be Created

_(none)_

---

## Changes

1. **VERIFY** that `ProjectTask.project` is a ForeignKey to `Project` ✓
2. **VERIFY** that `ProjectTask.assigned_to` is a ForeignKey to `User` ✓
3. **VERIFY** that `ProjectTask.status` has choices: 'Open', 'In-Progress', 'Closed' ✓
4. **VERIFY** that `Project` has reverse relation to `ProjectTask` via related manager ✓
5. **VERIFY** no schema migration is required — all fields and relationships are present ✓

---

## Tests

Add unit tests in `tests/api/models/test_project.py`:

- [ ] `ProjectTask.project` correctly references `Project`
- [ ] `ProjectTask.assigned_to` correctly references `User`
- [ ] `ProjectTask.status` choices include 'Open', 'In-Progress', 'Closed'
- [ ] `Project.tasks.count()` returns correct number of related tasks
- [ ] `status` field rejects invalid values
- [ ] `due_at` and `completed_at` are nullable and properly handled

---

## Scope Declaration

```yaml
allowed_files:
  - apps/api/models/project.py
  - tests/api/models/test_project.py

forbidden_operations:
  - CREATE new model
  - CREATE migration
  - MODIFY schema
```