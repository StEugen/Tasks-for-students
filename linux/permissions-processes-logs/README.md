# Laboratory: Linux Permissions, Processes, and Logs

## Goal

Investigate the Linux controls and runtime evidence a security specialist uses every day: identities,
permissions, process metadata, open sockets, and system logs.

Perform this laboratory on a disposable VM. 

## Learning outcomes

- interpret users, groups, mode bits, ownership, and `umask`;
- identify unsafe permissions without changing the whole system;
- inspect process ancestry, command lines, executable paths, and open sockets;
- correlate a process with log and network evidence;
- collect evidence while minimizing changes to the source systems 

## Part 1 — Identity and permissions

1. Record the output and purpose of `id`, `whoami`, `groups`, and `getent passwd "$USER"`.
2. Create a private laboratory directory. Record its owner, group, and symbolic/numeric mode.
3. Create files under `umask 022`, `027`, and `077`; compare resulting permissions.
4. Demonstrate the effect of user/group/other read, write, and execute bits on:
   - a regular file;
   - a directory.
5. Create one intentionally over-permissive file inside the lab directory, detect it with `find`,
   then correct only that file.
6. Explain why recursive `chmod 777` is not troubleshooting—it is surrender with extra keystrokes.

### Questions

- Why does execute permission mean something different for a directory?
- Why do newly created files normally not become executable even with a permissive `umask`?
- What risks arise from world-writable files and directories?
- What problem does the sticky bit solve on shared directories?

## Part 2 — Process investigation

Use a temporary Python HTTP server as the process under investigation. It runs as the current user and
must listen only on loopback, so it is not exposed to other machines.

### Start the process

1. Confirm that TCP port `8080` is not already in use:

```bash
ss -ltn | grep ':8080'
```

No output means that no TCP listener was found on that port. If the port is occupied, select another
unprivileged port above `1024` and use it consistently throughout the investigation.

2. In the first terminal, create a dedicated directory and a harmless page:

```bash
mkdir -p "$HOME/linux-security-lab/process-demo"
cd "$HOME/linux-security-lab/process-demo"
printf '%s\n' 'Linux process investigation laboratory' > index.html
```

3. Start the server in the foreground:

```bash
python3 -m http.server 8080 --bind 127.0.0.1
```

Leave this terminal open. Do not press `Ctrl-C` yet. The process should listen only on
`127.0.0.1:8080`; binding it to `0.0.0.0` would unnecessarily expose it to other interfaces.

4. Open a second terminal and prove that the service responds locally:

```bash
curl http://127.0.0.1:8080/
```

The process being investigated is the `python3 -m http.server` process—not `curl`, the terminal, or a
random system daemon that looked interesting and had the poor luck to be running nearby.

### Investigate the process

1. Locate the server with `ps` and `pgrep`. Record how you distinguished the correct process from
   unrelated Python processes.
2. Identify its PID, PPID, user, start time, elapsed time, state, and complete command line.
3. Draw its process ancestry using `pstree` or `ps --forest`.
4. Inspect its `/proc/PID/` entries for command line, executable, current directory, status, and file
   descriptors. Do not assume every entry is readable without privilege.
5. Prove with `ss` that the process listens on TCP loopback port `8080`. Correlate the socket with its PID.
6. Make another `curl` request and observe what changes temporarily in the socket state and server output.
7. Save relevant evidence before stopping the service.
8. Send `SIGTERM` to the server PID from the second terminal and verify that both the process and listening
   socket disappear. Compare graceful termination with `SIGKILL` conceptually. Do not use `SIGKILL`
   unless the teacher asks you to demonstrate it on a disposable process.
9. Keep the laboratory files until the report and hashes are complete. Delete them only as a separate,
   explicit cleanup step after teacher review.

### Questions

- Why can a deleted executable or log file remain visible through `/proc/PID/fd/`?
- What does a zombie process represent?
- Why is PPID useful during incident investigation?
- Why is a process name alone weak evidence of legitimacy?

## Part 3 — Logs and evidence

1. Determine whether the VM uses `systemd-journald`, traditional text logs, or both.
2. Query logs for the current boot and for one selected service.
3. Filter by time range, priority, and service unit.
4. Export a small relevant time window into the lab directory.
5. Record SHA-256 hashes of exported evidence and create `evidence-notes.md` containing:
   - collection time in UTC;
   - source command;
   - hostname;
   - analyst username;
   - evidence hash;
   - interpretation and limitations.
6. Verify the hash after copying the evidence file to another directory.

Do not claim that a copied log is forensic evidence without documenting collection and integrity.

## Incident investigation report

Use the shared [security laboratory report template](../../templates/security-lab-report-template.md).
Write one incident investigation report instead of separate disconnected answer files.

Required adaptations:

- Scope: identify the VM, user context, permitted privilege level, process, files, socket, and time window.
- Timeline: record discovery, evidence collection, analysis, teacher authorization, and any containment.
- Evidence: include permission listings, process ancestry, selected `/proc` data, listening sockets,
  exported logs, and integrity hashes.
- Findings: use formal findings for security weaknesses such as excessive permissions or unjustified
  exposure; use observations for process facts that are not inherently insecure.
- Conclusion: state whether the service appears expected, suspicious, or inconclusive and explain confidence.
- Recommendations: distinguish immediate containment from longer-term hardening.

Suggested evidence structure:

```text
evidence/
├── E-001-system-identity.txt
├── E-002-permissions.txt
├── E-003-process-tree.txt
├── E-004-proc-observations.txt
├── E-005-listening-sockets.txt
├── E-006-journal-extract.txt
├── E-007-hashes.txt
└── E-008-containment-retest.txt
```

Laboratory questions should be answered through the methodology, observations, findings, and appendix.
Collect evidence before containment and make the approval point visible in the timeline.

## Completion criteria

- permission conclusions are demonstrated on lab-owned files only;
- process claims are tied to PID, ancestry, executable, and socket evidence;
- evidence collection records source, UTC time, host, analyst, and hash;
- observation is completed before containment;
- the report distinguishes facts, interpretation, uncertainty, and recommended action;
- containment decisions and retests are visible in the evidence-backed timeline.
