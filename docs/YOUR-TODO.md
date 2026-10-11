# CareCircle: your to-do list

Deadline: **23 Oct 2026, 12:00 PM PT = 24 Oct, 12:30 AM IST**. Plan to submit on the **evening of 23 Oct (IST)**.

Status: the app, safety logic, MCP server (with elicitation), tests (29), evals and docs are **done**. Everything below needs you: accounts, keys, publishing and your voice. Steps marked 📱 can be done from your phone.

---

## 1. 📱 Blind test sentences (5 minutes, do this first)
Without looking at the project's test files, send me **10–15 sentences an elderly person might say** about:
- a suspicious call or message (some real scams, and a couple of harmless ones)
- how they feel (some serious, some minor)
- their pills ("did I take…", "I took…", "I forgot…")

Mix in Hinglish if you like. This is the honest accuracy test, and I'll report the score whatever it is.

## 2. AWS identity for Bedrock (laptop, ~10 minutes)
You're currently logged in as **root**. Create a limited user instead:
1. AWS console → **IAM → Users → Create user** → name `carecircle-dev` → **no** console access.
2. Permissions → **Add inline policy** → JSON tab → paste:
   ```json
   {"Version":"2012-10-17","Statement":[{"Effect":"Allow","Action":["bedrock:InvokeModel","bedrock:InvokeModelWithResponseStream"],"Resource":"arn:aws:bedrock:us-east-1::foundation-model/amazon.nova-pro-v1:0"}]}
   ```
3. User → **Security credentials → Create access key** → "Command Line Interface".
4. On the laptop, run: `aws configure --profile carecircle-dev` → paste the key and secret → region `us-east-1` → output `json`.
   **Never paste the keys into chat.**
5. Recommended: enable **MFA on root**, and create an **AWS Budgets** alarm (e.g. $5).
6. Tell me "profile done". I'll switch the app to Bedrock, run both evaluations on Nova Pro, and fix what it gets wrong.

## 3. GitHub repo (laptop, ~5 minutes)
Pick one:
- **Easy:** on github.com → **New repository** → name `carecircle` → **Public** → don't add a README or license (we already have them) → send me the URL. I'll push. Git may open a browser login window on the laptop the first time.
- **Or:** run `winget install GitHub.cli`, then `gh auth login`, then tell me. I'll create and push the repo myself.

## 4. Deploy for a phone/judge link (laptop, ~15 minutes, after step 3)
Follow `docs/deploy.md`:
1. IAM **role** `carecircle-ec2`, using the same policy as step 2.
2. Launch an **EC2** instance: Amazon Linux 2023, t3.small, ports 80/443 open, that role attached, with `deploy/ec2-user-data.sh` (with your repo URL) as user data.
3. After about 5 minutes, open `https://<ip-with-dashes>.sslip.io` on your phone. Send me the link and I'll check it.
   (This script hasn't been tested yet. If something fails, send me the error.)

## 5. Demo video (after 2 and 4)
- Follow `docs/demo-script.md` (2:50). Click **Reset demo (10:00)** before recording.
- Upload to **YouTube** (public; English; under 3 minutes) and send me the link.

## 6. 📱 Product feedback
Open `docs/product-feedback.md`. It's a draft from my notes, so rewrite it in your own words. The form asks for *your* experience with the tools.

## 7. Devpost submission (23 Oct, evening IST)
- [ ] Track: **Alexa+**. Mini-challenge: **AWS Builder** (Bedrock is used in the code). Optional: Open Source.
- [ ] Project description (I'll draft it from the README; ask me).
- [ ] Repository URL (public), demo video URL (public).
- [ ] Product feedback for every tool/API/SDK used (MCP SDK, Amazon Bedrock/Nova, Web Speech API).
- [ ] State clearly: **independent Alexa+-style simulation, not connected to an Echo device**.
- [ ] After submitting, open the public submission page and check every link.

---

**What I'll do as soon as you finish each step:**
- Step 1 → run your sentences and report the accuracy honestly.
- Step 2 → run on Bedrock, compare with the rule-based agent, and fix gaps.
- Step 3 → push the repo and check it looks right.
- Step 4 → check the live site.
- Step 7 → draft the description and fields.
