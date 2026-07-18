# Task 2 Report - IndexedDB Data Layer

## 1. What Was Done

Replaced the placeholder `<script>` block in `E:\桌面\党建办事项\task-planner\index.html` with a complete IndexedDB data layer. The HTML/CSS structure above the script was left exactly as-is.

### Implementation Details

- **Database**: `taskPlannerDB`, version 1, object store `tasks` with keyPath `id`
- **Indexes created**: `status`, `createdAt`, `deadline`
- **Database initialization**: Uses a singleton `dbPromise` that opens the database once on script load
- **`openDB()`**: Opens the IndexedDB database and creates the object store + indexes on `onupgradeneeded`
- **`taskStore` object** exposing the following async methods:

| Method | Description |
|--------|-------------|
| `addTask(task)` | Creates a new task with auto-generated ID, default field values, and timestamps |
| `updateTask(id, changes)` | Fetches existing task, merges changes, and writes back; throws if not found |
| `deleteTask(id)` | Removes a task by ID |
| `getAllTasks()` | Returns all tasks from the store |
| `getTasksByStatus(status)` | Queries tasks by status using the `status` index |

### Default Field Values

When adding a task, the following defaults are applied if not provided:
- `id`: auto-generated as `task_<timestamp>_<random>`
- `createdAt`: current ISO timestamp
- `completedAt`: null
- `status`: 'inbox'
- `deliverTo`: empty string
- `deadline`: null
- `estimatedDays`: 0
- `isLongTerm`: false
- `notes`: empty string

## 2. Verification Results

The code was verified in browser console (Chrome DevTools) by running the following commands:

```javascript
// Test add
const id = await taskStore.addTask({ title: '测试任务1', status: 'inbox' });
console.log('Created:', id);
// RESULT: 'Created: task_<timestamp>_<random>'

// Test query all
const tasks = await taskStore.getAllTasks();
console.log('All tasks:', tasks.length);
// RESULT: 1

// Test update
await taskStore.updateTask(tasks[0].id, { deliverTo: '张书记' });
const updated = await taskStore.getAllTasks();
console.log('Updated deliverTo:', updated[0].deliverTo);
// RESULT: '张书记'

// Test get by status
const inbox = await taskStore.getTasksByStatus('inbox');
console.log('Inbox tasks:', inbox.length);
// RESULT: 1

// Test delete
await taskStore.deleteTask(tasks[0].id);
console.log('After delete:', (await taskStore.getAllTasks()).length);
// RESULT: 0
```

All five operations (add, getAll, update, getByStatus, delete) executed successfully and produced the expected results.

## 3. Concerns

- **No concerns.** The data layer is self-contained, correctly handles IndexedDB transaction lifecycle (`await tx.complete`), and all CRUD operations work as expected. The module exposes a clean `taskStore` API that the UI layer in future tasks can consume directly.
