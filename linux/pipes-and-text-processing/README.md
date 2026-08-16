# Laboratory: Pipes and Text Processing

## Goal

Learn to treat command output as data. You will combine small Linux tools to answer security questions
from synthetic logs without modifying the original evidence.

## Learning outcomes

You should be able to:

- explain standard input, standard output, and standard error;
- use pipes and redirections deliberately;
- filter, transform, count, and rank text records;
- distinguish `>`, `>>`, `2>`, `2>&1`, and `|`;
- build a readable pipeline and verify intermediate results.

## Data

Use the files in [`data/`](data/):

- `auth.log` — synthetic SSH authentication events;
- `access.log` — synthetic HTTP access events.

The addresses and usernames are documentation examples. They do not identify real systems.

## Part 1 — Streams and redirection

Create a working directory and copy the two data files into it. Preserve the originals.

1. Display each file with `cat`, `less`, `head`, and `tail`. Explain when each tool is appropriate.
2. Count lines, words, and bytes with `wc`.
3. Redirect a successful command to `summary.txt`.
4. Run a command against a nonexistent file and redirect only its error to `errors.txt`.
5. Append another result to `summary.txt` without overwriting it.
6. Send standard output and standard error to separate files.

Explain the file descriptors `0`, `1`, and `2`.

## Part 2 — Authentication log questions

Build pipelines using combinations of `grep`, `cut`, `tr`, `sort`, `uniq`, `wc`, `head`, and `tee`.
Do not answer these by manually counting lines.

1. How many failed SSH login events are present?
2. Which source IP generated the most failed attempts?
3. Which usernames were attempted? Produce a unique, sorted list.
4. How many successful logins occurred, and from which addresses?
5. Show only events between `09:04:00` and `09:08:59`.
6. Save the ranked failed-source list to `failed-sources.txt` while also displaying it.
7. Create a pipeline whose final output is exactly:

```text
FAILED=<number>
ACCEPTED=<number>
```

## Part 3 — Web access log questions

1. Count responses by HTTP status code.
2. List the five most requested paths.
3. Rank client IP addresses by request count.
4. Display only server-error responses (`5xx`).
5. Find requests whose user-agent field contains `curl`.
6. Calculate the total bytes returned. Use `awk` for arithmetic.
7. Produce `suspicious-web.txt` containing requests for paths that include `.env`, `wp-login`,
   `phpmyadmin`, or `/admin`.

For every answer, save the command and a one-sentence interpretation.

## Part 4 — Pipeline engineering

Choose one pipeline containing at least four commands.

1. Run and verify each stage separately.
2. Draw it as:

```text
input -> command 1 -> command 2 -> command 3 -> output
```

3. Explain what data shape enters and exits every stage.
4. Replace one stage with a different tool while preserving the final result.
5. Compare `grep PATTERN file` with `cat file | grep PATTERN`. Which is clearer here and why?

## Log-analysis report

Use the shared [security laboratory report template](../../templates/security-lab-report-template.md).
Write the submission as a defensive log-analysis report.

Adapt the template as follows:

- Scope: the supplied synthetic `auth.log` and `access.log`; no external hosts are to be contacted.
- Methodology: collection, integrity check, filtering, aggregation, validation, and interpretation.
- Findings: report significant patterns such as concentrated authentication failures, suspicious path
  discovery, access-control responses, or repeated server errors. Do not label every count a vulnerability.
- Observations: document normal successful activity and text-processing behavior that provides context.
- Recommendations: connect each recommendation to an observed pattern and explain how its effect could
  be verified.
- Limitations: explicitly state that synthetic logs, limited fields, and lack of host context restrict attribution.

Required evidence:

```text
evidence/
├── E-001-input-hashes.txt
├── E-002-auth-summary.txt
├── E-003-failed-sources.txt
├── E-004-web-status-summary.txt
├── E-005-suspicious-web.txt
├── E-006-errors.txt
└── E-007-pipeline-validation.md
```

Put complete commands and selected output in Appendix A. The main report should communicate what the
data indicates, why it matters, confidence level, and what additional data would be needed. Place
answers to stream and pipeline questions in the methodology or appendix where they support the analysis.

## Completion criteria

- originals remain unchanged;
- results are derived with pipelines rather than manual counting;
- redirections do not accidentally overwrite prior work;
- every submitted pipeline can be explained stage by stage;
- each analytical conclusion references evidence and states its confidence or limitations;
- significant patterns are written as findings with proportionate, testable recommendations.
