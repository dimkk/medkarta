# План проекта

## Current scope

Owner asked to establish Медкарта, select an OSS foundation, and use MapleGPT as the subsequent iPhone bridge. Registration and research are the current deliverables; deployment, importing real records and implementing the bridge are subsequent tasks.

1. Register private repository, Obol project card, Vikunja board and documentation-only knowledge scope. Acceptance: live project/task URLs and scoped recall provenance.
2. Record OSS decision and exact version. Acceptance: primary sources, alternatives, license and unverified integration gates documented.
3. Validate YourPHR prototype on synthetic records. Acceptance: FHIR + attachment import, no repeated-import duplicates, correction/deletion, export and restore; verify maintained API and ARM64 build. Runtime placement and storage plan precede deployment.
4. Implement Medkarta authenticated ingestion and provenance. Depends on prototype gate. Acceptance: subject isolation, idempotency, validation, deletion and no health data in logs.
5. Integrate MapleGPT opt-in sync. Depends on task 471 and ingestion. Acceptance: explicit consent, partial permissions, lock/offline/retry, deduplication, deletion and device verification.
6. Add document collection and reviewed extraction. Preserve originals, issuer/date/units; extracted claims link to a page and remain unverified until reviewed.

No invented due dates. Full infrastructure promotion, public ingress and paid services require their separate concrete approval. Minimal registration can be rolled back by archiving its card/board and disabling documentation indexing while preserving the private repository; no medical records exist here to migrate.
