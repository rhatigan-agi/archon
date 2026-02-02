# Phase 3 — Implement Task Management UI

**Intent**: INTENT_007  
**Phase Type**: frontend  
**Depends On**: Phase 2

---

## Grounding (Existing State)

**USER RESOLUTION — MANDATORY TARGET**:

- `apps/frontend/components/ProjectDetailModal.tsx` — **User selected this file**
- This is the ONLY allowed frontend entry point for this intent

**UI Anchor Resolution**:

| Anchor | Status | Resolution |
|--------|--------|------------|
| `project_detail_view` | Not found | User provided: `ProjectDetailModal.tsx` |
| `todo_list_for_projects` | Not found | Will be introduced in this file |

**API Dependency** (from Phase 2):

- `GET /api/project-tasks/?project_id=<id>` — Returns tasks for project
- `POST /api/project-tasks/` — Creates new task
- `PATCH /api/project-tasks/<id>/` — Updates task
- `DELETE /api/project-tasks/<id>/` — Deletes task

---

## Acceptance Criteria Addressed

- [x] Add task list rendering to `ProjectDetailModal.tsx`
- [x] Display task title, description, status, and due date
- [x] Implement "Add Task" button with form
- [x] Enable status filtering (Open/In-Progress/Closed)
- [x] Implement task editing functionality
- [x] Populate task list from backend API

---

## Files to Modify

- `apps/frontend/components/ProjectDetailModal.tsx` — **MANDATORY (user resolution)**

## Files That May Be Created

- `apps/frontend/components/TaskListItem.tsx`
- `apps/frontend/components/TaskFormModal.tsx`
- `apps/frontend/hooks/useProjectTasks.ts`

---

## Changes

### In `ProjectDetailModal.tsx`:

1. **IMPORT** and render `TaskListItem` components for each task
2. **ADD** "Add Task" button that opens `TaskFormModal`
3. **ADD** status filter dropdown (Open / In-Progress / Closed)
4. **IMPLEMENT** inline editing of task title, description, status
5. **IMPLEMENT** status toggle (click badge to cycle: Open → In-Progress → Closed)
6. **FETCH** tasks from `/api/project-tasks/?project_id=<id>` on modal open
7. **UPDATE** task list state after create/update/delete

### In `TaskListItem.tsx` (new):

1. **DISPLAY**: title, description, status badge (color-coded), due date, assigned user
2. **DISPLAY**: edit/delete icons
3. **EMIT**: onEdit, onDelete, onStatusChange events

### In `TaskFormModal.tsx` (new):

1. **FORM FIELDS**: title (required), description, status (dropdown), due date (picker), assigned to (selector)
2. **ON SUBMIT**: POST to `/api/project-tasks/` with `project: <current_project_id>`
3. **ON CANCEL**: Close without mutation

### In `useProjectTasks.ts` (new):

1. **HOOK**: Fetch, create, update, delete tasks via API
2. **CACHING**: Use SWR or React Query, keyed by `project_id`
3. **ERROR HANDLING**: Toast on API failure

---

## Tests

### Unit Tests (`tests/frontend/components/ProjectDetailModal.test.tsx`):

- [ ] Renders task list when tasks are provided
- [ ] "Add Task" button opens form modal
- [ ] Status filter changes displayed tasks
- [ ] Clicking status badge cycles through statuses
- [ ] Editing task triggers API call

### Hook Tests (`tests/frontend/hooks/useProjectTasks.test.ts`):

- [ ] `fetchTasks(projectId)` calls correct endpoint
- [ ] `createTask(projectId, data)` POSTs with correct payload
- [ ] `updateTask(taskId, data)` PATCHes correctly
- [ ] `deleteTask(taskId)` DELETEs correctly

### E2E Tests (`tests/e2e/project-tasks.spec.ts`):

- [ ] Open project modal → tasks load
- [ ] Add new task → appears in list
- [ ] Change status to Closed → `completed_at` auto-set
- [ ] Filter by "Open" → only open tasks visible

---

## Scope Declaration

```yaml
allowed_files:
  - apps/frontend/components/ProjectDetailModal.tsx
  - apps/frontend/components/TaskListItem.tsx
  - apps/frontend/components/TaskFormModal.tsx
  - apps/frontend/hooks/useProjectTasks.ts
  - tests/frontend/components/ProjectDetailModal.test.tsx
  - tests/frontend/hooks/useProjectTasks.test.ts
  - tests/e2e/project-tasks.spec.ts

forbidden_operations:
  - MODIFY apps/api/*
  - MODIFY any file outside apps/frontend/
  - CREATE new API endpoints
```