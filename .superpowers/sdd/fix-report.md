# Fix Report: Task Planner Critical Bugs

**Date:** 2026-07-18  
**Files:** `task-planner/index.html`, `task-planner/sw.js`

## Bug 1: Notification icon path broken
**Status:** FIXED  
**File:** `task-planner/index.html` (lines 1493, 1563, 1584), `task-planner/sw.js` (line 6)  
**Change:** All `icon: '/task-planner/icon-192.png'` references changed to `icon: '/task-planner/icon.svg'`. Added `'/task-planner/icon.svg'` to the ASSETS array in sw.js so the service worker caches it.

## Bug 2: postponeTask accepts any input as deadline
**Status:** FIXED  
**File:** `task-planner/index.html` (lines 1434-1438)  
**Change:** Added date validation using `new Date(newDeadline)` and `isNaN(d.getTime())` check. Invalid input now shows a toast error: "日期格式无效，请输入如 2026-07-25 的格式".

## Bug 3: postponeTask uses setTimeout to revert status
**Status:** FIXED  
**File:** `task-planner/index.html` (lines 1430-1451)  
**Change:** Removed the intermediate `status: 'postponed'` assignment and the `setTimeout` callback. The function now only updates the deadline and keeps the task status as-is (typically `'in_progress'` or `'todo'`).

## Bug 4: HTML onclick async functions swallow errors
**Status:** FIXED  
**File:** `task-planner/index.html` (lines 1416-1428, 1430-1451)  
**Change:** Wrapped both `completeTask` and `postponeTask` function bodies in `try/catch` blocks. On error, they log to console and show a toast: "操作失败，请重试".

## Bug 5: serviceWorker.ready unhandled rejections
**Status:** FIXED  
**File:** `task-planner/index.html` (lines 1489-1496, 1559-1567, 1580-1588)  
**Change:** Wrapped all three `navigator.serviceWorker.ready` calls inside `try { ... } catch {}` blocks in `checkInboxReminders`, morning `sendCheckinNotification`, and evening `sendCheckinNotification`.

## Bug 6: HTML default times vs JS fallback mismatch
**Status:** FIXED  
**File:** `task-planner/index.html` (lines 996, 1001)  
**Change:** Updated HTML `value` attributes: morning from `09:00` to `08:30`, evening from `17:00` to `18:00`. Now matches the JS fallback defaults in `scheduleCheckins()` and `getSettings()`.
