# OSS foundation decision — 2026-09-18

## Recommendation

Use [YourPHR](https://github.com/jwilleke/yourphr) for the first integration prototype. It is a self-hosted personal-record application and continuation of Fasten; GPL-3.0. Keep upstream attribution and license when copying/modifying its code. This repository currently includes no upstream code.

GitHub API verified: not archived; 11 stars; latest push 2026-09-17; release v3.5.0 published 2026-09-14. Evaluation pin: `c15962479bc56ce7981ead35ee4e63c12fc3c668` (v3.5.0); inspected main: `de8f0a92ce61b90a348a34bdf79ee09b1ab48c55`. The small maintainer base is a continuity risk.

| Candidate | Fit | Decision |
|---|---|---|
| YourPHR, GPL-3.0 | Personal medical records, FHIR import, existing viewing UI | First prototype; validate exact release |
| [Fasten OnPrem](https://github.com/fastenhealth/fasten-onprem), GPL-3.0 | Same original product concept | Archived in GitHub; do not start a new fork here |
| [Medplum](https://github.com/medplum/medplum), Apache-2.0 | FHIR application platform, APIs and components | Fallback if YourPHR fails ingestion/export/maintenance gates; more product UI work |
| [OpenHealth](https://github.com/OpenHealthForAll/open-health), AGPL-3.0 | AI assistant using personal health information | Not selected as authoritative clinical archive; potential UX reference |

Medplum and OpenHealth licenses and active/archive flags checked through GitHub API; last pushes respectively 2026-09-18 and 2026-01-06. No benchmark or deployment test has been performed.

## Verified capabilities and limits

YourPHR README describes personal use and FHIR R4 bundle import. Source tree includes DocumentReference viewers and attachment UI. Their presence does not prove end-to-end PDF upload, OCR or attachment export works in the chosen release. Roadmap still tracks manual records and repeat-import deduplication. It does not establish a supported general FHIR REST write endpoint or built-in Apple Health connector. Do not assume either.

Documentation contains stale Go/compose/TLS descriptions alongside newer v3 material. Pin the release and inspect its actual server, migrations, API and container manifest before writing deployment files. Do not run README curl scripts blindly. Authentication, encryption, ARM64 support, backup/restore and import deletion behavior remain acceptance gates, not verified claims.

## Prototype gates

1. Install the pinned release in an approved isolated worker environment with synthetic data.
2. Import a synthetic FHIR R4 bundle containing Patient, Observation and DocumentReference, with an attached synthetic report; inspect units and timestamps.
3. Repeat import and apply a correction/deletion; verify identity/dedup semantics and source traceability.
4. Export and restore the records and attachments into a fresh instance; compare contents.
5. Establish supported ingestion API or a narrow authenticated adapter. Do not mutate private DB tables as the integration contract.
6. If essential gates fail without a small maintainable patch, evaluate the same fixture on Medplum before committing to a large fork.

Primary evidence: [README](https://github.com/jwilleke/yourphr/blob/de8f0a92ce61b90a348a34bdf79ee09b1ab48c55/README.md), [roadmap](https://github.com/jwilleke/yourphr/blob/de8f0a92ce61b90a348a34bdf79ee09b1ab48c55/docs/Roadmap.md), [release](https://github.com/jwilleke/yourphr/releases/tag/v3.5.0).
