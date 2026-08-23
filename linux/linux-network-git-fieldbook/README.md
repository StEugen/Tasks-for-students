# Laboratory: Linux Network Fieldbook with Git

## Why this laboratory comes next

You already know how to create, move, rename, inspect, and edit files in a Linux directory tree. In this
laboratory, that directory becomes a real Git repository and its contents become a small technical
fieldbook about your Linux host and network.

This is the bridge between basic terminal work and the later laboratories on pipelines, Bash scripting,
processes, logs, packet analysis, and firewalling.

## Goal

Create a clean, evidence-backed Git repository that documents one Linux VM: its operating-system
identity, files, interfaces, addresses, routes, name resolution, listening sockets, and safe connectivity
tests.

The repository must show not only the final files, but also a meaningful history of how the work was
created, reviewed, corrected, and released.

## Learning outcomes

After completing the laboratory, you should be able to:

- organize a Linux project with directories, files, relative paths, and hidden files;
- distinguish a command, its output, and an interpretation of that output;
- inspect interfaces, IP addresses, routes, neighbors, DNS configuration, and listening sockets;
- explain the difference between loopback, a local address, a default gateway, and a remote address;
- initialize and inspect a Git repository;
- stage selected changes and create small, meaningful commits;
- use branches, diffs, merges, tags, and `.gitignore` safely;
- publish a repository without leaking credentials, private keys, tokens, or unnecessary personal data.

## Prerequisites

- Your own disposable Linux VM running at home.
- The basic filesystem laboratory
  [`building-a-boilerplate`](../building-a-boilerplate/README.md) completed.
- Git and the `iproute2` tools available. You may install packages with `sudo` when needed.
- A GitHub account is needed only for the publishing part. All earlier work can be completed locally.

## Suggested effort

Plan for **6–10 hours** over two or three sessions:

1. repository setup and host inventory;
2. network mapping and bounded connectivity tests;
3. branch, merge, recovery, report, and GitHub release.


Record the Linux distribution and the Git version in your report. Do not paste authentication tokens or
private repository URLs into evidence.

## Scope and safety

### In scope

- your own disposable Linux VM;
- loopback addresses;
- the VM's own addresses and interfaces;
- its configured default gateway;
- files created inside your laboratory repository;
- a GitHub repository owned by you.

You are allowed to use `sudo` inside this VM. Before every privileged command, explain why ordinary-user
access is insufficient and inspect the command for unintended changes. Take a VM snapshot before any
network, firewall, package, or system-configuration change so recovery remains pleasantly boring.

### Out of scope

- scanning subnets or unrelated hosts;
- testing public IP addresses selected at random;
- changing routes, DNS, firewall rules, or interface configuration;
- committing passwords, API tokens, SSH private keys, browser data, complete environment dumps, or
  unredacted personal information.

## Required repository

Create this repository under a dedicated directory in your home directory. Choose a sensible repository
name such as `linux-network-fieldbook`.

Your final repository should contain at least:

```text
linux-network-fieldbook/
├── .gitignore
├── README.md
├── docs/
│   ├── host-inventory.md
│   ├── network-map.md
│   └── git-journal.md
├── evidence/
│   ├── E-001-host-identity.txt
│   ├── E-002-interfaces.txt
│   ├── E-003-routes.txt
│   ├── E-004-name-resolution.txt
│   ├── E-005-listening-sockets.txt
│   ├── E-006-connectivity.txt
│   └── E-007-release-verification.txt
└── report/
    └── assessment-report.md
```

Create the structure yourself. Do not copy another student's repository or use a generator that hides the
commands from you.

## Part 1 — Create and initialize the project

1. Create the repository directory and the required subdirectories and files.
2. Confirm your location before initializing Git. Explain why running `git init` in the wrong directory
   can be confusing.
3. Initialize the repository and select `main` as the primary branch.
4. Configure your Git author name and email at repository scope if they are not already correct. Do not
   change global configuration on a shared machine.
5. Use `git status` to identify untracked files.
6. Write a short project purpose, scope, and safety statement in `README.md`.
7. Create `.gitignore` entries for at least:
   - editor swap or backup files;
   - a `private/` directory;
   - temporary files produced during the lab.
8. Create an ignored test file inside `private/`. Prove with Git tooling that it is ignored, then delete
   it as a separate cleanup step.
9. Make the first commit. The commit should contain the structure and documentation—not secrets and not
   an unexplained pile of unrelated files.

### Questions

- What is the difference between the working tree, staging area, and repository history?
- Why does `git add .` deserve inspection before use?
- What does `.gitignore` not protect after a sensitive file has already been committed?
- Why should identity configuration be repository-local on a shared training machine?

## Part 2 — Build a Linux host inventory

Collect only information needed for this laboratory. For each command, record where it was run, why it
was run, whether it changed the system, and what the important output means.

Investigate:

1. current user, hostname, kernel, and Linux distribution;
2. current working directory and repository tree;
3. ownership and permissions of the repository and report file;
4. filesystem usage for the filesystem containing the repository;
5. current date in UTC and local time zone.

Store selected raw output in the appropriate evidence file. Write the explanation in
`docs/host-inventory.md`; do not turn the document into a landfill of terminal output.

Before staging, inspect the change with `git status` and `git diff`. Commit the host inventory separately
from the initial structure.

### Questions

- Which evidence identifies the operating system, and which identifies only the kernel?
- What can file permissions prove? What can they not prove about who actually read a file?
- Why is a complete environment-variable dump inappropriate evidence?
- Which inventory commands changed the system, if any?

## Part 3 — Map the local network

Without changing network configuration, investigate:

1. interfaces and their state;
2. IPv4 and IPv6 addresses and prefix lengths;
3. the routing table and default route;
4. the neighbor table;
5. configured hostname-resolution sources;
6. listening TCP and UDP sockets visible to your ordinary user.

Use standard Linux documentation and command help to identify suitable commands. Expected tool families
include `ip`, `ss`, `getent`, and either `resolvectl` or the resolver files used by the VM. If a tool is
unavailable, document the limitation rather than inventing output.

Create `docs/network-map.md` containing:

- a table of interfaces, addresses, states, and purposes;
- a simple ASCII topology showing the VM, its network, and its default gateway;
- a section explaining name resolution;
- a table of relevant listening sockets;
- facts separated from interpretations and unknowns.

If the repository will be public, redact hardware addresses and private addressing details that are not
needed to prove a conclusion. Record every redaction in the evidence register.

### Questions

- Why does `127.0.0.1` not reach another machine?
- What does an address prefix such as `/24` describe?
- Why can a host reach its local subnet without sending traffic through the default gateway?
- What is the difference between a listening socket and an established connection?
- Does an open listening socket automatically prove that another host can reach it? Explain.
- Why might the neighbor table be empty before local communication occurs?

## Part 4 — Perform bounded connectivity tests

Run only these bounded categories of tests:

1. test loopback reachability;
2. test one address assigned to the VM;
3. test the configured default gateway, if it responds;
4. resolve one well-known documentation hostname using the system resolver;
5. if the VM has Internet access, make one normal HTTPS request to a documentation site you selected
   for this test—do not scan it.

For each test, document:

- target and why it is authorized;
- expected result;
- actual result;
- whether failure proves the target is down;
- relevant limitations, such as ICMP filtering or unavailable Internet access.

Store concise evidence in `E-006-connectivity.txt`. A failed ping is a result, not permission to escalate
into subnet scanning.

Commit the network map and connectivity evidence as a coherent change.

## Part 5 — Use Git as an engineering tool

### A. Inspect history

1. Display the concise commit history.
2. Compare the current work with the previous commit.
3. Select one file and inspect its change history.
4. Explain the difference between an untracked, modified, staged, and committed file.

### B. Work on a branch

1. Create a branch named `docs/network-review`.
2. Improve `docs/network-map.md` on that branch—for example, correct an unclear explanation or add a
   missing limitation.
3. Review both the unstaged and staged diff before committing.
4. Commit the review with a meaningful message.
5. Return to `main`, compare the branches, and merge the reviewed documentation.
6. Delete the merged local branch only after proving its work is present on `main`.

### C. Experience and resolve one safe conflict

1. Create a temporary practice branch.
2. Change the same dedicated practice line in `docs/git-journal.md` differently on `main` and the
   practice branch.
3. Attempt the merge and inspect the conflict markers.
4. Resolve the conflict manually so the final text is accurate.
5. Stage and commit the resolution.
6. In `docs/git-journal.md`, explain what `ours`, `theirs`, and the final resolved content represented in
   this specific merge.

Do not create conflicts in evidence files merely for drama. Git already supplies enough drama without
corrupting your records.

### D. Recover from a harmless mistake

Demonstrate both cases using a disposable line in `docs/git-journal.md`:

1. restore an unwanted unstaged edit;
2. unstage a staged edit while preserving it in the working tree.

Record what happened and verify the final file. Do not rewrite published history.

## Part 6 — Review and publish to GitHub

Before publishing:

1. inspect every tracked filename;
2. inspect the final diff and concise history;
3. search the repository manually for secrets and personal data;
4. confirm that `private/` and temporary files are ignored;
5. ensure the working tree is clean;
6. create an annotated tag named `v1.0` with a useful message;
7. record the tag, final commit identity, and clean status in `E-007-release-verification.txt`.

Create a new empty GitHub repository. Do not initialize the remote with a README or license if those
already exist locally. Add it as `origin`, verify the remote URL, and publish `main` and the `v1.0` tag.

Use a **private** GitHub repository by default. Never place a personal access token in the remote URL,
shell history, documentation, screenshot, or repository file. Use a credential helper, SSH agent, or
browser-based login.

After publishing:

- open the GitHub repository and verify that the expected files and commits are present;
- confirm that ignored/private test content is absent;
- add the repository URL to the submission message, not to raw evidence if it exposes private metadata;
- do not force-push; this laboratory does not require rewriting remote history.

If GitHub access is unavailable, submit the complete local repository and document publishing as a
limitation. Local Git objectives can still pass.

## Assessment report

Copy and adapt the shared
[`security laboratory report template`](../../templates/security-lab-report-template.md) to
`report/assessment-report.md`.

Use it as a host and network inventory assessment report:

- **Scope:** identify the VM, repository, user privilege, authorized connectivity targets, and exclusions.
- **Environment:** assign an asset ID to the VM and record relevant Linux and Git details.
- **Methodology:** explain host inventory, network inventory, validation, Git review, and release phases.
- **Evidence:** register the seven required evidence files with UTC collection times and appropriate
  integrity notes.
- **Findings:** use formal findings only for supported security-relevant conditions, such as an
  unnecessarily exposed listener or sensitive tracked file. A normal interface is an observation.
- **Observations:** explain the host identity, routing, resolver, sockets, and Git state.
- **Recommendations:** make each action proportionate and testable.
- **Limitations:** state what ordinary-user access, isolation, filtering, or unavailable tools prevented
  you from proving.

## Required Git history

The final history must contain at least five meaningful commits whose purposes are distinguishable. It
must demonstrate:

- initial repository structure;
- host inventory;
- network inventory and bounded tests;
- documentation review performed on a branch;
- conflict resolution or final report/release preparation.

Commit count alone does not earn credit. Five useful commits beat twenty commits named `update`, because
archaeology should not be required to understand yesterday's work.

## Submission

Submit:

1. the GitHub repository URL, if publishing was available;
2. the final commit identity and `v1.0` tag;
3. `report/assessment-report.md`;
4. all seven evidence files;
5. a clean `git status` result;
6. a short submission note stating limitations, privileged actions, and any deviations from the lab.

Do not submit Git credentials, the contents of `.git/`, VM images, or unrelated home-directory files.

## Completion criteria

The laboratory is complete when:

- the repository has the required structure and contains no known secrets or unrelated personal data;
- host and network claims are supported by labeled evidence;
- the network topology distinguishes interfaces, local networks, and the default route;
- connectivity tests remain inside the stated authorization boundary;
- the student can explain the working tree, staging area, commits, branches, merges, tags, and remotes;
- Git history contains small, meaningful milestones and one reviewed branch merge;
- the student has demonstrated a safe conflict resolution and recovery from harmless edits;
- the final working tree is clean and `v1.0` identifies the submitted state;
- the report distinguishes facts, interpretations, findings, and limitations;
- the GitHub repository is verified after publishing, or lack of publishing access is documented.

## Self-review prompts

Demonstrate these without reading prepared answers:

1. how to tell whether a change is untracked, unstaged, staged, or committed;
2. which route would be used for the default gateway and why;
3. why a listening socket may still be unreachable remotely;
4. how `.gitignore` differs from removing a secret from repository history;
5. how they know the submitted tag and GitHub content match their local work;
6. one conclusion they could not prove and what additional evidence would be needed.
