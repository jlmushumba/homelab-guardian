# Homelab Guardian — Offline Development Roadmap

**Project:** Self-hosted backup, monitoring, and notification system  
**Developer:** Jean Louange Mushumba  
**Mentorship approach:** Build independently; request review after each phase.  
**Primary stack:** Python, Bash, Linux, SSH/Tailscale, Restic, systemd, SMTP/msmtp; web dashboard later.

## Project goal
Protect important laptop files with encrypted backups stored on the homelab, monitor system health, and send actionable email notifications. The laptop initiates backups; the homelab stores backups and runs server monitoring.

## How to use this file offline
- Open this `.md` file in any text editor or Markdown viewer.
- Mark a task complete by changing `- [ ]` to `- [x]`.
- Work on **one phase at a time**. Keep your own code, research notes, and decisions in the repository.
- After each checkpoint, record the evidence and problems encountered in the progress log at the end.
- **Never commit passwords, App Passwords, SSH private keys, tokens, or backup credentials.**

---

## Phase 1 — Project foundation

- [ ] Initialize a Git repository.
- [ ] Design a clear project directory structure.
- [ ] Create a README describing the problem, solution, and goals.
- [ ] Set up a Python virtual environment.
- [ ] Identify dependencies and system requirements.
- [ ] Document the architecture: what runs on the laptop versus the homelab.

**Checkpoint / definition of done:** The repository is organized, documented, and ready for development. You can explain how the components will communicate.

**Evidence to capture:** Repository tree, README, architecture sketch.

## Phase 2 — System monitoring engine

- [ ] Collect CPU utilization.
- [ ] Collect RAM utilization (accounting for available memory, not just free memory).
- [ ] Collect disk utilization.
- [ ] Collect uptime and hostname.
- [ ] Check essential services (Docker, Nginx, Tailscale), allowing for services not installed.
- [ ] Represent metrics in a predictable structured format (for example, JSON).
- [ ] Handle missing data, permissions errors, and command failures.

**Checkpoint / definition of done:** Monitoring runs on the homelab and produces accurate, machine-readable measurements.

**Evidence to capture:** Sample redacted JSON output and comparison with Linux commands.

## Phase 3 — Alert management

- [ ] Define configurable thresholds.
- [ ] Detect warning and critical states.
- [ ] Require problems to persist before alerting (avoid short spikes).
- [ ] Suppress repeated notifications for the same ongoing incident.
- [ ] Detect recovery and reset alert state correctly.
- [ ] Save alert state so restarts do not create duplicate alerts.

**Checkpoint / definition of done:** Simulated threshold crossings trigger one alert for a sustained incident and a recovery notification when normal conditions return.

**Evidence to capture:** Test scenarios, sample alert-state transitions, results.

## Phase 4 — Notification system

- [ ] Use the working `msmtp` configuration securely.
- [ ] Send emails from the dedicated homelab sender address.
- [ ] Create clear warning, critical, and recovery message templates.
- [ ] Include helpful context (host, issue, timestamp, observed values).
- [ ] Handle delivery errors and log failures without exposing credentials.

**Checkpoint / definition of done:** A real test alert arrives in the personal inbox and failures are recorded safely.

**Evidence to capture:** Redacted email example and delivery test result.

## Phase 5 — Backup management

- [ ] Verify secure SSH/Tailscale connectivity from laptop to homelab.
- [ ] Choose source folders and exclusion rules.
- [ ] Configure a Restic repository on the homelab.
- [ ] Create encrypted, versioned backups from the laptop.
- [ ] Track backup status, timestamp, and snapshot history.
- [ ] Restore test files and verify their contents.
- [ ] Handle unavailable network, insufficient storage, and interrupted backups.

**Checkpoint / definition of done:** A test file backs up successfully, can be restored to a different location, and matches the original.

**Evidence to capture:** Redacted backup summary, snapshot list, restore verification.

**Safety note:** Test on a disposable directory first. Do not enable destructive retention or pruning until you have proven restoration works. A homelab-only backup is not a complete disaster-recovery strategy; retain another copy of irreplaceable files.

## Phase 6 — Automation

- [ ] Create systemd service units for scheduled jobs.
- [ ] Create and enable systemd timers.
- [ ] Schedule monitoring checks on the homelab.
- [ ] Schedule backups on the laptop.
- [ ] Track missed backups and notify the user to decide whether to run them.
- [ ] Record useful logs and make failures diagnosable.

**Checkpoint / definition of done:** Monitoring and backups run at expected times, including defined behavior when the laptop was off or connectivity was unavailable.

**Evidence to capture:** Timer status, journal entries, test of a missed schedule.

## Phase 7 — Web dashboard

- [ ] Define a small Python API for metrics and statuses.
- [ ] Build a responsive HTML/CSS/JavaScript interface.
- [ ] Show recent system metrics and service status.
- [ ] Show backup history and the latest successful backup.
- [ ] Show active and resolved alerts.
- [ ] Restrict access (Tailscale and authentication); avoid unsafe remote shell execution.

**Checkpoint / definition of done:** An authorized device can view a reliable, usable dashboard over the private network.

**Evidence to capture:** Screenshots, API response samples, security decisions.

## Phase 8 — Testing, deployment, portfolio

- [ ] Add unit tests for parsing, thresholds, and state transitions.
- [ ] Add integration tests for backup and notification flows.
- [ ] Test restore procedures and important failure scenarios.
- [ ] Check permissions, secret handling, and least-privilege access.
- [ ] Document installation, configuration, troubleshooting, and recovery.
- [ ] Prepare architecture diagrams and screenshots.
- [ ] Write a portfolio-ready README with demonstrated results and limitations.
- [ ] Record a short demo or prepare repeatable demonstration steps.

**Final checkpoint / definition of done:** You can demonstrate a monitored homelab, a tested encrypted backup/restore cycle, automated scheduling, and actionable notifications.

---

## Engineering decisions already made

| Topic | Decision |
|---|---|
| Backup direction | Laptop → homelab (push over SSH/Tailscale) |
| Backup execution | Runs on laptop |
| Storage | Encrypted Restic repository on homelab |
| Missed backup | Notify user and request action rather than silently skipping |
| Resource alerts | Alert only after sustained threshold violation |
| Language | Python for application logic; Bash/Linux tools for system integration |
| Email delivery | msmtp via dedicated Gmail account |
| Automation | systemd services and timers |
| Dashboard | Later phase, not part of the initial MVP |

## Progress log — fill this in as you work

| Date | Phase / task | What I finished | How I tested it | Blocker / next step |
|---|---|---|---|---|
| YYYY-MM-DD | Phase 1 |  |  |  |
| YYYY-MM-DD |  |  |  |  |
| YYYY-MM-DD |  |  |  |  |
| YYYY-MM-DD |  |  |  |  |

## Phase review template

When asking for mentor feedback, share:

1. **Phase completed:**
2. **What I built:**
3. **Evidence / tests:**
4. **Design decisions and trade-offs:**
5. **What didn't work / what I learned:**
6. **What I want reviewed:**

**Rule:** Prefer understanding, working tests, and safe defaults over adding lots of features quickly.
