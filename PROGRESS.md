# CareCircle — Alexa+ Hackathon progress

Deadline: 23 Oct 2026 12:00 PT (24 Oct 00:30 IST). Submit on 23 Oct evening IST.

## Decisions (2026-10-09)
- Project: **CareCircle** — voice-first eldercare agent + family dashboard.
- Real MCP server (TS SDK 1.32.1, protocol 2025-11-25, Streamable HTTP).
- Web client = real MCP client (initialize / tools/list / tools/call), Alexa-style voice UI (Web Speech API).
- Model: Amazon Bedrock (Converse API, tool use). Deterministic fallback if no creds.
- Hosting: AWS. AWS CLI not installed yet on this PC — user needs AWS account + credentials.

## Differentiators
1. Double-dose guard (agent checks the medication log before recording a dose).
2. Refill forecasting (pills left vs schedule, reorder never happens without confirmation).
3. Family escalation — symptoms/missed doses → caregiver alert feed + daily digest.
4. Scam guard — classify a suspicious call/message, alert family.
5. Confirmation gate for consequential writes; full MCP trace + audit log; eval suite.

## Phases
- [ ] P1 MCP server + SQLite + tools + tests
- [ ] P2 Agent loop (Bedrock + fallback)
- [ ] P3 Voice web client (elder view) + family dashboard (PWA)
- [ ] P4 Eval set (30 cases) + docs
- [ ] P5 Deploy to AWS, demo video, submit

## Next step
P1: scaffold `server/`.
