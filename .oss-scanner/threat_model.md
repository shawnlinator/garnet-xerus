# Threat model

## What this project does and where untrusted input enters
An in-memory data store (C, a Redis fork). Untrusted input: the RESP protocol (commands and
arguments), persistence (RDB) files, and Lua scripts. The protocol parser and command handlers
are the core untrusted surfaces.

## Components that matter most / least
- most: the RESP protocol parser and command handlers, plus the Lua engine boundary.
- less: modules, tests.

## How to exercise it
Send crafted RESP commands to a running server; load crafted RDB files; run edge-case Lua.

## How you rate severity
- Memory-safety bugs (overflow/UAF) reachable via the protocol: critical (RCE).
- Logic bugs: high. DoS: medium.

## Anything to leave alone
- Do not report issues only reachable via privileged commands as high severity.
