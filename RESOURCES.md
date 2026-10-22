# Resources

## Tools

- [Model Context Protocol](https://modelcontextprotocol.io/) — specification and security best practices.
- [mcp-scan](https://github.com/invariantlabs-ai/mcp-scan) — inspect and pin MCP tool descriptions.
- [bubblewrap](https://github.com/containers/bubblewrap), [gVisor](https://gvisor.dev/), [Firecracker](https://firecracker-microvm.github.io/) — sandboxing layers.
- [Dev Containers](https://containers.dev/) — reproducible containerised workspaces.
- [gitleaks](https://github.com/gitleaks/gitleaks), [TruffleHog](https://github.com/trufflesecurity/trufflehog) — secret scanning.
- [Thinkst Canarytokens](https://canarytokens.org/) — decoy credentials.
- [Semgrep](https://semgrep.dev/), [CodeQL](https://codeql.github.com/), [Bandit](https://github.com/PyCQA/bandit) — static analysis.
- [OSV-Scanner](https://github.com/google/osv-scanner), [pip-audit](https://github.com/pypa/pip-audit) — dependency vulnerability checks.
- [zizmor](https://github.com/zizmorcore/zizmor) — GitHub Actions static analysis.
- [StepSecurity Harden-Runner](https://github.com/step-security/harden-runner) — CI runner egress control.
- [OpenSSF Scorecard](https://github.com/ossf/scorecard) — repository security posture checks.

## Papers and write-ups

- Greshake et al., "Not what you've signed up for: Compromising Real-World LLM-Integrated Applications with Indirect Prompt Injection", AISec 2023.
- Pearce et al., "Asleep at the Keyboard? Assessing the Security of GitHub Copilot's Code Contributions", IEEE S&P 2022.
- Spracklen et al., "We Have a Package for You! A Comprehensive Analysis of Package Hallucinations by Code Generating LLMs", USENIX Security 2025.
- Boucher and Anderson, "Trojan Source: Invisible Vulnerabilities", USENIX Security 2023.
- Invariant Labs blog posts on MCP tool poisoning and GitHub MCP exfiltration (2025).
- Johann Rehberger, [Embrace The Red](https://embracethered.com/blog/) — agent prompt-injection research.

## Learning

- [OWASP Top 10 for LLM Applications](https://genai.owasp.org/llm-top-10/).
- [GitHub Actions security hardening guide](https://docs.github.com/en/actions/security-for-github-actions/security-guides/security-hardening-for-github-actions).
- Simon Willison's writing on prompt injection, [simonwillison.net](https://simonwillison.net/).
