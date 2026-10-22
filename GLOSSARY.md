# Glossary

- **Agentic coding assistant:** an LLM-driven tool that reads a codebase, runs commands and edits files to complete a task.
- **Indirect prompt injection:** instructions placed in content the model reads (files, issues, tool output) rather than typed by the user.
- **MCP (Model Context Protocol):** an open protocol for exposing tools and data sources to LLM clients.
- **Tool poisoning:** hiding instructions in an MCP tool's description or metadata so the model follows them.
- **Rug pull (MCP):** a server changing its tool descriptions or behaviour after the user approved it.
- **Confused deputy:** a privileged component tricked into misusing its authority on behalf of an attacker.
- **Lethal trifecta:** private data access plus untrusted content plus an external communication channel.
- **Slopsquatting:** registering package names that LLMs hallucinate so that generated install commands fetch attacker code.
- **Egress allow-list:** network policy that permits outbound connections only to named destinations.
- **Canary token:** a decoy credential or file whose use triggers an alert.
- **ASCII smuggling:** hiding text in Unicode tag characters that are invisible in most UIs but readable by models.
- **`pull_request_target`:** a GitHub Actions trigger that runs with base-repository secrets, risky when combined with untrusted PR content.
