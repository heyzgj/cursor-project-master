# Task Template for CPM Kanban System

## File Naming Convention
- **Filename:** `T-<ID>.md` (e.g., `T-1.md`, `T-42.md`)
- **Location:** Based on status:
  - `project/tasks/backlog/` → **Inbox** column (new/unassigned tasks)
  - `project/tasks/todo/` → **Next Up** column (ready to start)
  - `project/tasks/in_progress/` → **Running** column (actively worked on)
  - `project/tasks/done/` → **Done** column (completed tasks)
  - `project/tasks/blocked/` → **Blocked** column (tasks waiting for external input)
  - `project/tasks/obsolete/` → **Obsolete** column (tasks no longer needed)

## Required Format

```markdown
# <TaskTitle>

**Epic:** <EpicName>  
**Type:** feature | bug | test | research | design  
**Effort:** 1 | 2 | 3 points  
**Status:** inbox | next-up | running | done | blocked | obsolete  
**Attempts:** 0  
**UI Review:** true | false  
**Design Review:** pending | approved | not-required  

## Description
<Brief description of what needs to be accomplished>

## Acceptance Criteria
- [ ] <Specific, testable requirement>
- [ ] <Another requirement>
- [ ] <Final requirement>

## Dependencies
- <TaskID> (<Optional description>)

## Context Binding
- `/docs/PRD.md` (if relevant to requirements)
- `/docs/TECH_SPEC.md` (if relevant to technical implementation)
- `/src/components/specific-file.tsx` (only files directly relevant to this task)

## Checklist
- [ ] <Actionable step 1>
- [ ] <Actionable step 2>
- [ ] <Actionable step 3>
- [ ] <Final step>

## Notes
<Optional implementation notes, context, or considerations>

## Metadata
- **Created:** <timestamp>
- **Last Modified:** <timestamp>
- **Degraded Completion:** false
```

## Field Specifications

### **Epic** (Required)
- Format: `E-<ID> - <EpicName>` or descriptive name
- Example: `E-1 - UI Skeleton Implementation`

### **Type** (Required)
- `feature`: New functionality
- `bug`: Fix broken behavior  
- `test`: Add/improve testing
- `research`: Investigation or spike
- `design`: UI/UX design work

### **Effort** (Required)
- `1`: Small task (1-4 hours)
- `2`: Medium task (1-2 days)  
- `3`: Large task (2-3 days)

### **Status** (Auto-managed by file location)
- Determined by directory, not frontmatter
- File moves between directories change status

### **Attempts** (Auto-managed by 100-engine)
- Tracks number of failed execution attempts
- Reset to 0 on successful completion
- Used by 300-adaptation for failure resolution

### **UI Review** (Required for UI tasks)
- `true`: Task requires visual sign-off before implementation
- `false`: No UI review needed

### **Design Review** (Auto-managed)
- `pending`: Waiting for user approval
- `approved`: User has approved design
- `not-required`: No design review needed

### **Context Binding** (Required)
- List of files that must be loaded for this task
- Keep minimal to optimize token usage
- Only include directly relevant files

### **Acceptance Criteria** (Required)
- Must be testable and specific
- Written as checkboxes `- [ ]`
- Defines "done" for the task

### **Dependencies** (Optional)
- List other task IDs this depends on
- Format: `- T-<ID> (<description>)`

### **Checklist** (Required for UI tasks)
- Actionable implementation steps
- Interactive in Kanban UI
- Progress calculated from completion ratio

### **Degraded Completion** (Auto-managed)
- `true`: Task completed in degraded mode (e.g., without CI)
- `false`: Normal completion
- Used for tracking tasks that need validation when services recover

## Example Task

```markdown
# ImplementUserAuthentication

**Epic:** E-2 - User Management System  
**Type:** feature  
**Effort:** 3 points  
**Status:** next-up  
**Attempts:** 0  
**UI Review:** true  
**Design Review:** pending  

## Description
Implement secure user authentication with JWT tokens, including login, logout, and session management.

## Acceptance Criteria
- [ ] Users can log in with email/password
- [ ] JWT tokens are issued on successful login
- [ ] Protected routes require valid authentication
- [ ] Users can log out and invalidate tokens
- [ ] Session expires after 24 hours

## Dependencies
- T-15 (Database schema for users)
- T-16 (Password hashing utility)

## Context Binding
- `/docs/TECH_SPEC.md`
- `/docs/DESIGN_SPEC.md`
- `/src/lib/auth.ts`
- `/src/components/LoginForm.tsx`

## Checklist
- [ ] Set up JWT library and configuration
- [ ] Create login API endpoint
- [ ] Implement password validation
- [ ] Add JWT token generation
- [ ] Create logout endpoint
- [ ] Add authentication middleware
- [ ] Protect existing routes
- [ ] Add session expiration handling
- [ ] Write unit tests for auth flow
- [ ] Test with frontend integration

## Notes
Use bcrypt for password hashing. Consider refresh token strategy for production.
JWT secret should be in environment variables.

## Metadata
- **Created:** 2024-01-15T10:30:00Z
- **Last Modified:** 2024-01-15T10:30:00Z
- **Degraded Completion:** false
```

## AI Agent Guidelines

When creating tasks:
1. **Always include all required fields**
2. **Make acceptance criteria specific and testable**
3. **Break large tasks into smaller ones (max effort: 3)**
4. **Use consistent Epic naming**
5. **Add detailed checklists for complex tasks**
6. **Reference dependencies by task ID**
7. **Place files in correct directory for desired status**
8. **Include Context Binding with only relevant files**
9. **Set appropriate UI Review and Design Review flags**
10. **Initialize Attempts to 0 for new tasks**

## Status Flow
```
backlog/ (Inbox) → todo/ (Next Up) → in_progress/ (Running) → done/ (Done)
                                                           ↘ blocked/ (Blocked)
                                                           ↘ obsolete/ (Obsolete)
```

Tasks move through directories as they progress through the workflow.
