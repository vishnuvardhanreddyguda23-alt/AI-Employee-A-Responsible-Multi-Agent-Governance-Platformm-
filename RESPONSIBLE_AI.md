# Responsible AI

## Purpose
The platform governs AI employees before sensitive actions execute.

## Principles
- Authorization is deterministic and backend-enforced; an LLM does not decide permissions.
- Risk decisions return LOW/MEDIUM/HIGH/CRITICAL with reasons.
- Sensitive actions can require a human decision.
- Decision traces store evidence such as inputs, workflow states, retrieved sources, tools, policy checks, risk, approvals, final output and evaluation results — never private model chain-of-thought.
- Outputs that cannot be reliably verified may be marked `UNVERIFIED`.
- Evaluation measures task success, grounding, correctness, format compliance, latency, tool accuracy and safety policy compliance.
- Privacy and security controls are documented rather than promised as perfect.

## Limitations
No system here provides 100% hallucination detection, complete prompt-injection prevention, or absolute security. Human oversight remains required for high-impact operations.
