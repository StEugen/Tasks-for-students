# Laboratory: Bash Scripts for Log Triage

## Goal

Turn ad-hoc pipelines into a defensive Bash utility. The final program will validate its input,
analyze an authentication log, create a report, and return meaningful exit codes.

Use the synthetic `auth.log` from the pipes laboratory as input.

## Learning outcomes

You should be able to use:

- a shebang and executable permissions;
- variables, positional arguments, quoting, and command substitution;
- tests, `if`, `case`, loops, functions, and exit codes;
- `set -u` and `set -o pipefail` deliberately;
- `mktemp` and `trap` for safe temporary files;
- pipelines inside a maintainable script;
- `shellcheck` and controlled test cases.

## Rules

- Write the script as an ordinary user.
- Quote variable expansions unless you can explain why splitting is intended.
- Do not use fixed temporary paths such as `/tmp/report.txt`.
- Do not delete or modify the input log.
- Never use `eval`.
- A failed command must not silently produce a believable report.

## Part 1 — Small scripts

Create these scripts first:

1. `hello.sh`
   - prints the current username, hostname, and current working directory;
   - exits successfully.
2. `check-file.sh PATH`
   - reports whether `PATH` exists and whether it is a regular file, directory, readable, and writable;
   - prints usage and exits with code `2` if the argument is missing;
   - exits with code `1` if the path does not exist.
3. `count-pattern.sh PATTERN FILE`
   - validates both arguments and file readability;
   - prints the number of matching lines;
   - treats the pattern as text rather than a regular expression;
   - uses `--` where supported to prevent an argument beginning with `-` from becoming an option.

For every script, demonstrate the exit code with:

```bash
./script-name arguments
echo "$?"
```

## Part 2 — Main assignment: `log-triage.sh`

### Interface

```text
Usage: log-triage.sh [-n NUMBER] [-o REPORT] AUTH_LOG
```

Options:

- `-n NUMBER` — number of top failed source addresses to show; default `5`;
- `-o REPORT` — write the report to this path as well as standard output;
- `-h` — print help and exit successfully.

### Required behavior

The script must:

1. parse options with `getopts`;
2. require exactly one input file;
3. reject a missing, non-regular, or unreadable input;
4. reject `-n` values that are not positive integers;
5. never modify the input file;
6. calculate:
   - total lines;
   - failed authentication count;
   - accepted authentication count;
   - unique usernames used in failed attempts;
   - ranked source addresses for failed attempts;
7. print a human-readable report with a UTC generation time;
8. write the same complete report to `-o REPORT` when requested;
9. use functions to separate validation, analysis, and report generation;
10. return meaningful exit codes.

### Suggested exit-code contract

| Code | Meaning |
|---:|---|
| `0` | success |
| `1` | runtime or input error |
| `2` | command-line usage error |

Document your final contract in the script header.

## Part 3 — Safety and robustness

Use a unique temporary directory created with `mktemp -d`. Register cleanup immediately with `trap`.
Your cleanup function must quote its path and refuse to operate if the variable is empty.

Answer these questions:

1. What bug can `for item in $(command)` cause?
2. What is the difference between `"$@"` and `$*`?
3. Why is `[ -f "$file" ]` safer than `[ -f $file ]`?
4. What does `pipefail` change?
5. Why can `set -e` be surprising in conditionals and pipelines?
6. What attack can occur when a filename beginning with `-` is passed to a command?
7. Why should temporary files not use predictable names?

## Part 4 — Test matrix

Run and document at least these cases:

| Case | Expected behavior |
|---|---|
| valid auth log | report; exit `0` |
| missing argument | usage; exit `2` |
| nonexistent file | clear error; nonzero exit |
| directory as input | clear error; nonzero exit |
| `-n 0` | usage error |
| `-n abc` | usage error |
| output filename containing spaces | report written correctly |
| empty readable file | valid zero-count report |
| input filename beginning with `-` | handled safely or documented invocation with `--` |

Create test fixtures in a unique temporary directory. Cleanup should be a separate, visible test step.

## Part 5 — Review

1. Run `bash -n` against each script.
2. Run `shellcheck` against each script and explain any intentionally suppressed warning.
3. Read the script aloud, function by function. If a line cannot be explained, it is borrowed—not learned.

## Script security review report

Use the shared [security laboratory report template](../../templates/security-lab-report-template.md).
Submit the implementation together with a security engineering validation report.

The report must include:

- Scope: the four scripts, supported Bash environment, input log, and test boundaries.
- Environment: Bash and ShellCheck versions and relevant platform assumptions.
- Methodology: static syntax review, ShellCheck review, functional tests, negative tests, input-integrity
  verification, and cleanup verification.
- Findings: document each security or reliability defect discovered during development—for example,
  unquoted expansion, unsafe temporary paths, option injection, incorrect exit status, partial reports,
  or failed cleanup. Include the original behavior, impact, correction, and retest evidence.
- Test results: map every test case to expected result, actual result, exit code, and evidence ID.
- Limitations: identify untested shells, encodings, very large inputs, concurrency, or platform differences.
- Conclusion: state whether the final scripts satisfy the defined interface and safety contract.

Suggested submission:

```text
scripts-lesson-submission/
├── security-review-report.md
├── hello.sh
├── check-file.sh
├── count-pattern.sh
├── log-triage.sh
├── sample-report.txt
└── evidence/
    ├── E-001-bash-syntax.txt
    ├── E-002-shellcheck.txt
    ├── E-003-test-matrix.md
    ├── E-004-invalid-input-tests.txt
    ├── E-005-input-hashes.txt
    └── E-006-cleanup-verification.txt
```

A corrected defect is still valuable report content: record it as mitigated and show the retest. Do not
manufacture vulnerabilities merely to make the report look busy. Reality is allowed to be boring.

## Completion criteria

- all required valid and invalid cases are demonstrated;
- arguments and paths containing spaces are handled correctly;
- input data is unchanged;
- output is reproducible and exit codes match the documented contract;
- `bash -n` and `shellcheck` findings are addressed or justified;
- the report traces each test and defect to evidence;
- fixed defects include mitigation status and successful retest evidence.

## Optional challenge

Add `-j` to produce JSON without hand-building unsafe JSON strings. Use an installed JSON-aware tool such
as `jq`, document the dependency, and verify the output with `jq empty`.
