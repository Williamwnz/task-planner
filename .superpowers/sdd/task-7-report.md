# Task 7 Report: JSON Export/Import

**Date:** 2026-07-18
**File modified:** `E:\桌面\党建办事项\task-planner\index.html`

## Summary
Added JSON export/import functionality to the PWA task planner app. The code was appended before the closing `</script>` tag at the end of the file.

## Element ID Verification
| Expected ID | Actual ID in HTML | Match |
|---|---|---|
| `btn-export` | `btn-export` (line 830) | Yes |
| `btn-import` | `btn-import` (line 831) | Yes |

Both button IDs match the provided code exactly. No adjustments were needed.

## Code Added
- **Export handler:** Reads all tasks from IndexedDB via `taskStore.getAllTasks()`, serializes to JSON, triggers a file download named `task-planner-backup-YYYY-MM-DD.json`, and shows a toast.
- **Import handler:** Opens a hidden file input (`.json` filter). On file select:
  - Parses the JSON and validates it is an array.
  - Prompts the user with a confirm dialog:
    - **OK (确定):** Overwrite mode -- clears all existing tasks, then imports all from file.
    - **Cancel (取消):** Merge mode -- only adds tasks whose `id` does not already exist in the database.
  - Re-renders all zones after import.
  - Shows a success toast on completion or an error toast on failure.

## APIs Used
- `taskStore.getAllTasks()` -- read all tasks
- `taskStore.addTask(task)` -- insert a task
- `taskStore.deleteTask(id)` -- remove a task by ID
- `renderAllZones()` -- refresh the UI
- `showToast(msg)` -- display toast notification

## Verification Steps (Manual)
1. Open `index.html` in a browser.
2. Add a few test tasks via the + FAB.
3. Click the export button (header, left icon) -- verify a `.json` file downloads.
4. Delete all tasks (open each task detail and click delete, or use DevTools).
5. Click the import button (header, middle icon) -- select the downloaded JSON.
6. Choose "取消" (merge) -- verify tasks are restored.
