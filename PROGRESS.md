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

## Status 2026-10-10 (night, user away; autonomous work)
Done: MCP elicitation for approvals (session-based Streamable HTTP, SSE; agent relays to device; dashboard approval card;
voice yes/no), requested-refill state, fixed literal-backspace regex bug (\b) in agent.ts/ruleAgent.ts/App.tsx,
README rewrite, docs/demo-script.md, docs/product-feedback.md (DRAFT - user must review), deploy/ec2-user-data.sh +
docs/deploy.md (UNTESTED), device framing UI. 29 tests pass; eval 32/32; heldout 24/24 (contaminated).
Blocked on user: least-privilege AWS profile (then .env AWS_PROFILE + AGENT_MODE=bedrock -> run eval + heldout on Bedrock),
GitHub repo creation (public, MIT), deploy choice (EC2 script ready), blind test phrases, demo video recording.
Next for me after Bedrock run: fix whatever Bedrock gets wrong (evidence first), maybe server-side conversation memory.

## Status 2026-10-11
- Chat bug root cause: `aws login` (root) session expired -> CredentialsProviderError on every Bedrock call -> silent fallback
  to rule planner whose catch-all was the schedule. Fixed visibility (/api/agent/mode credential check, per-reply answered_by,
  server log line), partial-failure guard (no fallback after writes), real catch-all replies. +3 regression tests.
- New scenarios (server/eval/blind-2026-10-11.json, author-written, frozen before run): 12/15 with 2 CRITICAL (chest metaphor
  missed; "Neha fainted ... send money" -> false emergency + missed scam). Fixed generally; now 15/15 (no longer blind).
- 34 tests pass; eval 32/32; held-out 24/24. Pushed to https://github.com/Ohi-Me/CareCircle (main e89f194, owner LICENSE kept).
- AWS: no carecircle-dev profile; root login expired. App runs AGENT_MODE=rules (inline env) until user fixes creds.
- Proposed: also enter OpenCV AI Competition 2026 (deadline Oct 26 PT) with a pill-box vision check (OpenCV 5 = 5.0.0.93 on pip)
  as an MCP tool -> agentic vision. Rules checked: neither forbids multiple competitions; both licenses non-exclusive. Awaiting user go/no-go.
