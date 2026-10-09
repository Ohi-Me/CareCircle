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
- [x] P1 MCP server + SQLite + 16 tools (tests still TODO)
- [x] P2 Agent loop (Bedrock Converse code + rule fallback; Bedrock untested - no AWS creds yet)
- [x] P3 Voice web client + family dashboard + MCP trace (browser MCP client), PWA manifest
- [x] P4 17 tests pass, 30-case eval (rules 30/30 at 10:00 demo clock), README, Dockerfile, LICENSE
- [ ] P5 Deploy to AWS, demo video, submit

## Next step
Run: `cd server && npm start` (after `cd web && npm run build`) -> http://localhost:8787.
Next: user sets up AWS (Bedrock access + aws configure) -> run eval in bedrock mode -> deploy. Docker not installed locally; deploy options: install Docker Desktop + App Runner/ECR, or Lightsail/EC2 with Node. Then GitHub repo, demo video, Devpost submit.

## Review 2026-10-09 (evening) — decisions
Probe of 12 unseen requests: rule agent ~3/12. Critical misses: stroke signs, grandson-jail scam; false emergency on
"I don't have chest pain"; "It's okay to take it now" (unsafe claim); "this morning" dose stamped as now; hypoglycemia not escalated.
Reframe: **"Ask before you act"** — CareCircle is the pause between an older adult and a risky action, family in the loop.
Hero demo: scam call -> device says hang up -> Ravi's phone alert -> Ravi replies -> device speaks it; + duplicate-dose clarification.
Priority: (1) safety screen on every utterance (2) medication logic rewrite (3) two-way family loop (4) escalation ladder
(5) MCP elicitation for confirmations (stateful sessions) (6) held-out eval + scam precision/recall (7) server-side conversation
memory (8) SNS notifications (9) split-screen demo view. Held-out set is frozen BEFORE planner changes (docs/heldout).

## Status 2026-10-10 (early)
Done since review: safety screen (safety.ts/screen.ts), medication logic rewrite (possible_duplicate -> clarify ->
extra dose urgent), confirmation guard, held-out sets frozen + baseline (docs/heldout-baseline-rules.md) and after
(docs/heldout-after-safety-rules.md; contaminated - need blind set), family loop: escalation ladder Ravi->Neha->tell her
112 (family.ts), respond_to_alert / send_message_to_elder / announcements spoken on device, +3 min demo clock. 20 MCP tools, 26 tests.
AWS: user installed CLI 2.37, `aws login` as ROOT, Nova Pro verified in us-east-1. Local .env pins AGENT_MODE=rules until
user creates least-privilege profile `carecircle-dev` (IAM policy: bedrock:InvokeModel on amazon.nova-pro-v1:0).
Next: (a) Bedrock run of eval + heldout once profile exists (b) server-side conversation memory for LLM path
(c) MCP elicitation for confirmations (d) deploy (App Runner needs Docker; or EC2/Lightsail) (e) blind test phrases from user
(f) GitHub repo, demo video.
