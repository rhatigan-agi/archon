# [FEATURE] Implement Project Task Management UI and API Integration

**Intent**: INTENT_007  
**Generated**: 2026-02-01T16:12:34.891Z  
**Coverage**: backend ✓, frontend ✓

---

### Summary

Implement a TODO list feature for Projects by extending existing backend models and integrating with the ProjectDetailModal.tsx frontend component to display and manage project-related tasks.

---

### Backend Acceptance Criteria

- [ ] Extend `ProjectTask` API endpoints to support filtering tasks by `project_id`
- [ ] Ensure `ProjectTaskSerializer` properly serializes task status (Open/In-Progress/Closed) and related fields
- [ ] Verify that `Project` API endpoints include related tasks when fetching project details
- [ ] Confirm that existing `ProjectTask` model relationships are correctly utilized
- [ ] Ensure task status updates through API correctly reflect in the database

---

### Frontend Acceptance Criteria

- [ ] Add task list rendering to `apps/frontend/components/ProjectDetailModal.tsx`
- [ ] Display task title, description, status, and due date in the modal
- [ ] Implement "Add Task" button with form to create new ProjectTasks linked to the current project
- [ ] Enable status filtering (Open/In-Progress/Closed) for task list
- [ ] Implement task editing functionality within the modal
- [ ] Ensure task list is populated from the backend when viewing project details

---

### Technical Requirements

- Use existing `ProjectTask` model (`apps/api/models/project.py`)
- Leverage existing `status` field with choices: Open, In-Progress, Closed
- Reuse `assigned_to` field for task assignment
- Extend existing API endpoints in `project_views.py` (do not create new endpoints)
- Use `ProjectDetailModal.tsx` as the primary UI component (per user resolution)

---

### Grounding (from Discovery)

| Fact | Source | Confidence |
|------|--------|------------|
| `ProjectTask` model exists | `apps/api/models/project.py` | absolute |
| `status` field has correct choices | AST scan | absolute |
| `migration_required: false` | Pass 4 schema check | deterministic |
| Frontend target: `ProjectDetailModal.tsx` | User resolution | authoritative |

---

### Files Affected

**Backend:**
- `apps/api/models/project.py` (existing ProjectTask model — verify only)
- `apps/api/views/project_views.py` (extend existing endpoints)
- `apps/api/serializers/project_serializers.py` (verify serializer)

**Frontend:**
- `apps/frontend/components/ProjectDetailModal.tsx` (mandatory — user selected)

---

### Coverage Validation

```
Intent concerns:  [backend, frontend]
Issue addresses:  [backend, frontend]
Status: COMPLETE ✓
```