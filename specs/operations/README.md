# Operational Command Journal

Use one dated entry per task or operation in this directory when work inspects,
validates, plans, or changes the VPS, host, WireGuard interfaces, routes, or
firewall. This journal is the canonical detailed record of operational work;
task files contain only concise evidence and links.

For each command, record:

- date, task ID, purpose, working directory, and relevant sanitized context;
- the command or a safe description when the literal command would expose a secret;
- whether it is a read, verification, plan, failed attempt, or external write;
- explicit authorization for external writes; and
- a sanitized result and any correction made.

Never include private keys, tokens, real public IP addresses, endpoints,
device codes, raw state, firewall dumps, or secrets. No external operational
commands have been run for this project yet.
