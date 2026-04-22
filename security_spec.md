# Firebase Security Specification

## Data Invariants
- A `User` can only be created by an admin or if it matches the current `auth.uid`.
- A `Project` can only be managed (CUD) by admins.
- A `Team` can only be managed (CUD) by admins.
- A `Line` can only be managed (CUD) by admins.
- A `Task` can only be managed (CUD) by admins. 
- A `DailyReport` can only be created/updated by the `staffId` matching the reporter or an admin.
- Reports must belong to a valid `taskId` and `staffId`.
- Budget and numerical fields must be non-negative.
- All IDs must follow standard format `^[a-zA-Z0-9_\\-]+$`.

## The Dirty Dozen (Payloads to Reject)
1. User profile update with `role: 'admin'` by a non-admin.
2. Creating a `Task` with a negative budget.
3. Updating a `DailyReport` by a different staff member.
4. Injecting a 2MB string into a `Line` name.
5. Deleting a `Project` by a staff member.
6. Creating a `Task` with `startDate` after `endDate`.
7. Updating `ownerId` of a document after creation.
8. Listing all `User` profiles without being authenticated.
9. Creating a `DailyReport` for a non-existent `taskId`.
10. Using a 500-character string as a `projectId`.
11. Changing the `projectId` of a `Task` after it's been created.
12. Submitting a `DailyReport` with a future date.

## Relationship Mapping
- `Task` -> `Project`: A task implements marketing for a project.
- `Task` -> `Line`: A task is assigned to a line.
- `DailyReport` -> `Task`: A report tracks metrics for a specific task.
- `User` -> `Line`: A user belongs to a working line.
- `Line` -> `User` (Leader): A line is managed by a leader.
