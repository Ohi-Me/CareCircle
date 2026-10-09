# Demo video script (target 2:50)

Setup: desktop browser, window wide enough to show **her device** (left) and the **family dashboard** (right). Click **Reset demo (10:00)**. Turn on "Speak replies". Keep the MCP trace tab ready.

| Time | Screen | Say / do |
|---|---|---|
| 0:00–0:15 | Title, then the app | "Older adults get scam calls, forget whether they took a pill, and fall when nobody's there. Their family is often a city away. CareCircle is an Alexa+-style voice companion built on a real MCP server, and its rule is: ask before you act." |
| 0:15–0:50 | Left: tap the mic | **Asha:** "A man from the bank is still on the line asking for my OTP." Device: "**Hang up now.** This looks like a scam…" Right: an urgent alert appears for **Ravi**, with "escalates at 10:03". |
| 0:50–1:10 | Click **+3 min** | Nobody answered, so the alert **escalates to Neha** and the timeline shows it. "The family gets a deadline, not just a notification." |
| 1:10–1:30 | Right: as Neha, tap the quick reply "Don't share anything. I'll call you in 2 minutes." | Left: her device **speaks Neha's message**. Escalation stops. |
| 1:30–2:00 | Left | **Asha:** "I just took my blood pressure pill." → recorded. **Asha:** "I took my BP pill." → "My records show Amlodipine at 10:00. Is that the dose you mean, or did you take another one?" **Asha:** "I took another one." → urgent alert and pharmacist guidance. "It never says a dose is safe; it says what's recorded and asks." |
| 2:00–2:25 | Left | **Asha:** "Am I running low on any medicine?" A Yes/No card appears: "Send a refill request for 30 × Amlodipine…?" "This question comes from the MCP server itself, through MCP elicitation, so the AI can't approve anything on her behalf." Say "Yes". → Done; the Refills card shows ✓ Requested. |
| 2:25–2:45 | MCP trace tab | Point at: initialize (protocol 2025-11-25), tools/list (20 tools), the safety screen verdict, tools/call latencies, the elicitation question and answer. "Every step is a real JSON-RPC message over Streamable HTTP. The agent runs on Amazon Bedrock (Nova Pro), and the safety rules are deterministic code with tests and a held-out evaluation." |
| 2:45–2:50 | — | "CareCircle is an independent Alexa+-style simulation with sample data. It is not connected to an Echo device." |

Tips: record at 1080p. Do a dry run after **Reset demo (10:00)**, because the duplicate-dose beat needs the BP pill recorded first. If speech recognition mishears, type the line instead; the flow is identical.
