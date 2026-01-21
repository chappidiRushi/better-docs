---
id: bootstrap-application
title: Bootstrap
sidebar_position: 1
-------------------

```text


Write Component
   ↓ // JSX in function/class
JSX Compiled
   ↓ // Babel → React.createElement
Entry File Runs
   ↓ // index.tsx / main.tsx
Select DOM Container
   ↓ // document.getElementById
createRoot()
   ↓ // React root initialized
root.render(App)
   ↓ // Initial update scheduled
Fiber Root Created
   ↓ // HostRoot fiber
Lane Assigned
   ↓ // Priority decided
Scheduler Starts
   ↓ // When to work
──────── Render Phase ────────
   ↓ // Pure & interruptible
Root Component Called
   ↓ // App(props)
Hooks Initialized
   ↓ // useState / useEffect
Component Returns Elements
   ↓ // Virtual DOM nodes
Child Components Called
   ↓ // Depth-first
Hooks Called In Order
   ↓ // Position-based
Virtual DOM Tree Complete
   ↓ // Immutable
Fiber Tree Built
   ↓ // Work-in-progress
Reconciliation
   ↓ // Diff with current fiber
Effect Flags Collected
   ↓ // Placement / Update / Delete
Render Phase Ends
   ↓ // Can pause/restart
──────── Commit Phase ────────
   ↓ // Atomic, not interruptible
DOM Nodes Created
   ↓ // Host fibers only
DOM Mutations Applied
   ↓ // Minimal changes
Refs Attached
   ↓ // ref.current set
useLayoutEffect Runs
   ↓ // Sync, before paint
Browser Paints
   ↓ // Pixels rendered
useEffect Runs
   ↓ // Async, after paint
Event System Active
   ↓ // Synthetic events
App Interactive
   ↓ // Ready for updates
State Updates Trigger Loop Again

```
