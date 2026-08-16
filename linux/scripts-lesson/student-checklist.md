# Bash scripts lesson — student checklist

## Small scripts

- [ ] `hello.sh` prints the required environment information.
- [ ] `check-file.sh` validates its argument and returns documented exit codes.
- [ ] `count-pattern.sh` handles spaces and option-like arguments safely.

## Main script

- [ ] Has a Bash shebang and a short usage function.
- [ ] Uses `getopts` for `-n`, `-o`, and `-h`.
- [ ] Requires exactly one readable regular file.
- [ ] Quotes path and argument expansions.
- [ ] Separates validation, analysis, and output into functions.
- [ ] Uses `mktemp -d` rather than a predictable temporary path.
- [ ] Registers and verifies cleanup with `trap`.
- [ ] Does not modify the source log.
- [ ] Prints and optionally saves the complete report.
- [ ] Uses documented exit codes.

## Security review report

- [ ] Shared report template was copied and adapted as `security-review-report.md`.
- [ ] Scope, supported environment, assumptions, and limitations are explicit.
- [ ] Every test case records expected result, actual result, exit code, and evidence ID.
- [ ] Discovered defects record impact, correction, and retest status.
- [ ] Important claims refer to evidence files.
- [ ] Input-integrity and temporary-file cleanup evidence are included.
- [ ] Secrets and irrelevant raw output are absent.

## Verification

- [ ] Test matrix is recorded with actual and expected results.
- [ ] `bash -n` succeeds for every script.
- [ ] `shellcheck` findings are fixed or explained.
- [ ] Output file with spaces in its name works.
- [ ] Input hash before and after testing is identical.
- [ ] Temporary artifacts are removed in a separate cleanup step.
