# PostgreSQL Database Tables Reference

Complete reference of core database tables mapped by the FuraOJ v2.0 Rust backend.

## Table Catalog

### `auth_user`
| Column | Type | Constraints | Description |
| :--- | :--- | :--- | :--- |
| `id` | SERIAL | PRIMARY KEY | User unique identifier |
| `username` | VARCHAR(150) | UNIQUE, NOT NULL | Unique handle |
| `password` | VARCHAR(128) | NOT NULL | PBKDF2/SHA256 password hash |
| `email` | VARCHAR(254) | NOT NULL | User contact email |
| `is_staff` | BOOLEAN | NOT NULL | Staff administrator flag |
| `is_active` | BOOLEAN | NOT NULL | Active account flag |
| `is_superuser` | BOOLEAN | NOT NULL | Superuser privilege flag |
| `date_joined` | TIMESTAMPTZ | NOT NULL | Account creation timestamp |

### `judge_profile`
| Column | Type | Constraints | Description |
| :--- | :--- | :--- | :--- |
| `id` | SERIAL | PRIMARY KEY | Profile identifier |
| `user_id` | INTEGER | UNIQUE, FK `auth_user.id` | Associated user |
| `rating` | INTEGER | NOT NULL | Competitive rating |
| `points` | DOUBLE PRECISION | NOT NULL | Total accumulated points |
| `problem_count`| INTEGER | NOT NULL | Number of solved problems |

### `judge_problem`
| Column | Type | Constraints | Description |
| :--- | :--- | :--- | :--- |
| `id` | SERIAL | PRIMARY KEY | Problem unique ID |
| `code` | VARCHAR(64) | UNIQUE, NOT NULL | Problem unique code slug |
| `name` | VARCHAR(255) | NOT NULL | Problem title |
| `description` | TEXT | NOT NULL | Markdown statement with math |
| `time_limit` | DOUBLE PRECISION | NOT NULL | Execution time limit in seconds |
| `memory_limit`| INTEGER | NOT NULL | Execution memory limit in MB |
| `points` | INTEGER | NOT NULL | Maximum score points |
| `is_public` | BOOLEAN | NOT NULL | Public visibility |

### `judge_submission`
| Column | Type | Constraints | Description |
| :--- | :--- | :--- | :--- |
| `id` | SERIAL | PRIMARY KEY | Submission ID |
| `problem_id` | INTEGER | FK `judge_problem.id` | Target problem |
| `user_id` | INTEGER | FK `auth_user.id` | Submitting programmer |
| `language` | VARCHAR(32) | NOT NULL | Language runtime identifier |
| `source` | TEXT | NOT NULL | Solution source code |
| `status` | VARCHAR(16) | NOT NULL | Verdict status code (`AC`, `WA`, `QU`) |
| `time` | DOUBLE PRECISION | NULL | Execution runtime in seconds |
| `memory` | INTEGER | NULL | Peak memory used in KB |
| `points` | DOUBLE PRECISION | NULL | Earned points |
| `date` | TIMESTAMPTZ | NOT NULL | Submission submission time |
