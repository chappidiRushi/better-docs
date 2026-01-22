---

id: mongodb-backup-restore-dr
title: Backup, Restore, and Disaster Recovery
sidebar_position: 17
--------------------

# 17. Backup, Restore, and Disaster Recovery

> Data protection strategies to survive data loss, corruption, and cluster failures

---

## Logical Backups

> Exporting data at logical (document/collection) level

**Interview points**

* Uses `mongodump` / `mongorestore`
* BSON-based, schema-aware
* Slower, but portable

<details>
<summary>Examples (Node.js)</summary>

```js
// Trigger logical backup via child process
import { exec } from 'node:child_process';

exec('mongodump --uri="mongodb://user:pass@host:27017/app" --out ./dump');
```

</details>

---

## Physical Backups

> Copying raw database files from disk

**Interview points**

* Storage-engine specific
* Fast restore
* Requires filesystem-level consistency

<details>
<summary>Examples (Node.js)</summary>

```js
// Snapshot orchestration (example abstraction)
await adminDb.command({ fsync: 1, lock: true });
// take volume snapshot externally
await adminDb.command({ fsyncUnlock: 1 });
```

</details>

---

## Point-in-Time Recovery (PITR)

> Restoring data to an exact moment

**Interview points**

* Requires oplog
* Common in replica sets
* Combine snapshot + oplog replay

<details>
<summary>Examples (Node.js)</summary>

```js
// Check oplog window
const status = await adminDb.command({ replSetGetStatus: 1 });
status.members.forEach(m => console.log(m.optime));
```

</details>

---

## Restore Pitfalls

> Common restore-time failures and mistakes

**Interview points**

* Index rebuild cost
* UUID / namespace conflicts
* Version incompatibility
* Dropping admin/local by mistake

<details>
<summary>Examples (Node.js)</summary>

```js
// Dangerous restore example
exec('mongorestore --drop ./dump'); // Drops existing collections
```

</details>

---

## Notes

* Test restores regularly
* Backups without restore tests are useless

## Caveats

* Logical backups impact performance
* Physical backups are unsafe without fsync
* PITR increases storage and ops complexity
