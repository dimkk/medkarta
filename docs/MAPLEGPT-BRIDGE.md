# Apple Health → MapleGPT → Медкарта

Status: integration design, not a working sync. Owner requested this route on 2026-09-18.

## Integration boundary

MapleGPT is the intended native iOS bridge. The implementer must confirm the available HealthKit read layer with its maintainer. This specification is self-contained and does not require access to a private task board or checkout.

Medkarta sync is an opt-in destination, distinct from local summaries or sending a summary to chat. Reuse the HealthDataManager/query layer when available. Do not route records via conversation transcripts or overwrite concurrent MapleGPT work.

## Proposed flow

1. In MapleGPT, enable “Медкарта”, show exact private destination, data types and history window; obtain OS HealthKit read authorization and explicit destination consent. No write access to Apple Health.
2. Start with activity, heart, sleep and workouts defined for the initial read layer. Additional measurements are independently enabled later.
3. Initial bounded history import; later incremental reads using HKAnchoredObjectQuery, with an anchor per type/query scope. HKObserverQuery can wake the app; no fixed real-time delivery guarantee.
4. Send authenticated batches directly to the Medkarta ingestion adapter. Store credentials in Keychain. Persist a protected local queue, retain cursor only after durable server acknowledgement; retry with stable batch IDs and bounded backoff. Do not create a controller poller.
5. Adapter validates schema, sizes, units, subject binding and source provenance; uses a verified YourPHR import path. Selected measurements can become FHIR Observations; sleep/workouts need explicit mapping preserving original meaning. Not all Apple records are one generic Observation.

## Contract to implement

Versioned envelope: schema_version, opaque source installation ID, batch_id, selected type, collection window, records and deleted record IDs. Each record has stable HealthKit UUID, original type, start/end time with offset, value/unit or category, source metadata restricted to what provenance needs. Bind patient on the authenticated server credential, not on a client-supplied patient ID. Never include tokens or device serial numbers in payload diagnostics.

Separate raw samples from daily aggregates; summing all iPhone and Watch step samples can double-count. Define source priority and compare daily totals with HealthKit statistics. Tombstones must propagate deletions; retries must not recreate deleted records. Anchor reset or changed scope triggers a bounded reconciliation, not blind append.

Read denial is intentionally not distinguishable from no accessible data. Successful authorization sheet completion does not mean every type is readable. Locked-device unavailability must preserve last-sync timestamp, not replace values with zero. Background wake is best effort; on-demand foreground sync is the first acceptance path.

## Privacy and acceptance

No values in shared logs, tasks, knowledge or default LLM prompts. In-app consent must list destination and selected data scope; disabling sync stops new transfers and clears queued unsent data according to the documented policy. Server deletion and retention must be a visible separate action. Medical records need private access, encryption and tested restore before first real import.

Tests: empty/partial access, duplicate batches, lost ACK, pagination, revoked access, offline retry, locked phone, time-zone/DST boundary, iPhone+Watch overlap, deleted records, changed scope. Real-phone acceptance requires owner interaction with the HealthKit sheet; compare counts/totals for the same period and prove a repeated sync adds no duplicates.

Apple sources: [setup](https://developer.apple.com/documentation/healthkit/setting-up-healthkit), [authorization](https://developer.apple.com/documentation/healthkit/authorizing-access-to-health-data), [anchored queries](https://developer.apple.com/documentation/healthkit/executing-anchored-object-queries), [observer delivery](https://developer.apple.com/documentation/healthkit/executing-observer-queries).
