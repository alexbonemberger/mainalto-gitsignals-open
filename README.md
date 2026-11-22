# MainAlto GitSignals (public transport)

**State: SOURCE_ONLY / NOT_INSTALLED / NO_LOCAL_EXECUTION.** This repository is a transport for *requests and non-sensitive receipts*, not a command interpreter or an authority to run software.

## Boundary

- Controller: ChatGPT with GitHub write access, after operator-selected scope.
- Producer: local Qwen through the existing **private** ChatAutomation C# Windows runtime.
- Output: proposed `.cs` source in local quarantine; proposal delivered by existing Edge/UIA ChatAutomation.
- Approval: isolated candidate tests may be delegated to ChatGPT **only in a genuinely isolated, unprivileged environment**. Any additional Windows permissions, host code execution, installation, production mutation, or privileged effects require explicit operator approval bound to a source hash and effect class.
- This first increment **does not execute generated code**, run Qwen, install a worker, send UIA messages, or grant authorization merely by the presence of a file here.

## Request contract (v1)

Only files under `requests/<request_id>.json` with an exact single immutable ID, `schema: mainalto.gitsignals.request/1`, `operation: capability.propose`, `model: qwen3-vl:2b-instruct`, `chat`, `capability`, `objective`, and future `expires_utc` are eligible. ID and names use `[a-z0-9-]` and bounded lengths. The local consumer pins the Git commit/blob and verifies exact chat alias before any interaction. A changed request ID is a conflict, not an update.

`capability.propose` means **produce a review-only C# source candidate**. It must not be interpreted as an instruction to compile, execute, install, delete, send unrelated messages, or invoke shell commands. Prompt and model output are untrusted text.

The ChatAutomation branch carrying the runtime is isolated from the existing main, candidate chess-cycle and MainAlto R14-R25 artifacts. `qwen-autorepair-01` remains immutable.

## One-way events in initial delivery

- `requests/`: controller -> local C# receiver, one unique filename per request.
- Local `proposal.cs`, `request.json`, `receipt.json`: receiver journal in user-controlled state folder; **not** posted to this public repository.
- Edge/UIA: receiver -> controlling conversation, with request ID, source digest, full source when within size limit, and explicit `PROPOSAL_ONLY` status.
- Further authorizations and receipts are **not yet implemented**. They must not be inferred from Git comments, free text, or an unverified UIA click.

## Usage policy

No secrets, local user documents, private paths, authentication tokens, or privileged operation parameters in this public repository. Configure stop and target conversation locally. The local watcher is opt-in, not started by this repository. The host must confirm an operator-enabled session before it can send UIA messages. Any uncertain dispatch is terminal for that request; no automatic replay.

## Evidence levels

`SOURCE_COMMITTED` != `CI_PASS` != `WINDOWS_INSTALLED` != `UIA_MESSAGE_CONFIRMED` != `CAPABILITY_FUNCTIONAL`. Never promote a stage based only on a successful commit, build, or the model's assertion.
