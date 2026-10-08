# PostgreSQL Database Schema Invariance

A foundational invariant of FuraOJ v2.0 is **100% Schema Invariance**. The Rust backend directly connects to the existing PostgreSQL database schema without modifying table structures, column names, or relationships.

## Preserved Database Tables

The system maps all primary tables exactly as defined by the original 170+ migrations:

1. `auth_user`: User identity, staff status, and superuser flags.
2. `judge_profile`: Competitive ratings, points, and solved problem tallies.
3. `judge_problem`: Problem codes, statements, time limits, and memory limits.
4. `judge_submission`: Submissions, verdict strings, execution time, and memory usage.
5. `judge_submissiontestcase`: Granular per-test-case execution metrics.
6. `judge_contest`: Competitive contest definitions, schedules, and freeze flags.
7. `judge_contestproblem`: Problem association and point weighting in contests.
8. `judge_contestsubmission`: Submissions recorded during active contest rounds.
9. `judge_contestparticipation`: User registration and contest scores.
10. `judge_judge`: Registered judge worker identities and authentication keys.

## Data Types & Invariants

- All primary keys are 64-bit (`BIGINT`) or 32-bit (`INTEGER`).
- Timestamps use `TIMESTAMPTZ` with timezone awareness.
- Verdicts map directly to standard string codes: `AC`, `WA`, `TLE`, `MLE`, `CE`, `RTE`, `QU`.
