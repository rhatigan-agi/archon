# Phase 2 — Extend ProjectTask API Endpoints

**Intent**: INTENT_007  
**Phase Type**: backend  
**Depends On**: Phase 1

---

## Grounding (Existing State)

**Existing Endpoints** (from Discovery):

- `ProjectTaskViewSet` exists in `apps/api/views/project_views.py`
- `ProjectTaskSerializer` exists in `apps/api/serializers/project_serializers.py`

**Existing Models** (from Phase 1 grounding):

- `ProjectTask` model with `project`, `status`, `assigned_to`, `due_at`, `completed_at` fields

---

## Acceptance Criteria Addressed

- [x] Extend `ProjectTask` API endpoints to support filtering tasks by `project_id`
- [x] Ensure `ProjectTaskSerializer` properly serializes task status and related fields
- [x] Verify that `Project` API endpoints include related tasks when fetching project details
- [x] Ensure task status updates through API correctly reflect in the database

---

## Files to Modify

- `apps/api/views/project_views.py`
- `apps/api/serializers/project_serializers.py`

## Files That May Be Created

_(none)_

---

## Changes

### In `apps/api/views/project_views.py`:

1. **VERIFY** that `ProjectTaskViewSet` inherits from `ModelViewSet` and is registered in router
2. **ADD** filtering by `project_id` to `get_queryset()`:
   ```python
   def get_queryset(self):
       queryset = super().get_queryset()
       project_id = self.request.query_params.get('project_id')
       if project_id:
           queryset = queryset.filter(project_id=project_id)
       return queryset
   ```
3. **VERIFY** that PATCH/PUT on `ProjectTask` endpoint updates `status`, `due_at`, `assigned_to`, and `completed_at` correctly

### In `apps/api/serializers/project_serializers.py`:

1. **VERIFY** that `ProjectTaskSerializer` includes: `id`, `project`, `title`, `description`, `status`, `assigned_to`, `due_at`, `completed_at`, `created_at`
2. **VERIFY** that `status` field uses choice representation (string, not integer)
3. **VERIFY** that `project` field is read-only (prevent reassignment)
4. **ENSURE** `ProjectSerializer` includes `tasks` field (nested or ID-only) to satisfy frontend requirement

---

## Tests

Add integration tests in `tests/api/views/test_project_views.py`:

- [ ] `GET /api/project-tasks/?project_id=<uuid>` returns only tasks for that project
- [ ] `PATCH /api/project-tasks/<uuid>/` with `{"status": "Closed"}` updates database correctly
- [ ] `GET /api/projects/<uuid>/` includes `tasks` array with task details
- [ ] `POST /api/project-tasks/` with valid payload creates task and returns 201
- [ ] Attempt to PATCH `project` field → expect 400 or ignored (read-only)

Add serializer tests in `tests/api/serializers/test_project_serializers.py`:

- [ ] `ProjectTaskSerializer` outputs correct field structure
- [ ] `ProjectSerializer` includes `tasks` field
- [ ] `status` field serializes as string choice

---

## Scope Declaration

```yaml
allowed_files:
  - apps/api/views/project_views.py
  - apps/api/serializers/project_serializers.py
  - tests/api/views/test_project_views.py
  - tests/api/serializers/test_project_serializers.py

forbidden_operations:
  - CREATE new model
  - MODIFY apps/api/models/*
  - CREATE new endpoint file
```