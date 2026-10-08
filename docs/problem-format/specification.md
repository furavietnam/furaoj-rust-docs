# Problem Directory Specification

Problems in FuraOJ are stored as standalone folders indexed by problem code or ID.

## Directory Layout

```
problems/<problem_code>/
├── init.yml           # Problem configuration & limits
├── statement.md       # Markdown problem statement with KaTeX math
├── 1.in               # Test case 1 input
├── 1.out              # Test case 1 expected output
├── 2.in               # Test case 2 input
├── 2.out              # Test case 2 expected output
└── checker.cpp        # Optional custom evaluation checker
```

## Problem Manifest (`init.yml`)

```yaml
archive: aplusb
test_cases:
  - points: 20
    in: 1.in
    out: 1.out
  - points: 20
    in: 2.in
    out: 2.out
  - points: 60
    in: 3.in
    out: 3.out
```

Test case inputs and outputs are processed in ASCII/UTF-8 format with trailing whitespace normalization.
