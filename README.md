
# How to improve your chances of getting a bounty

---

## 1) Pick the *right* targets

* Choose programs with active bounty payments and clear scopes (HackerOne, Bugcrowd, Intigriti, vendor-run programs). New or poorly maintained programs often don’t pay.
* Focus on smaller / mid-size targets where triage teams are lean — they often pay faster and more reliably than huge vendors.
* Re-check program policy and allowed targets before testing (scope, excluded assets, reward ranges).

---

## 2) Hunt for high-impact classes, not just noisy low-value bugs

Prioritize vulnerabilities that lead to real consequence: account takeover (ATO), sensitive data disclosure, remote code execution, privilege escalation, chained auth bypasses, CSRF leading to state change, SSRF to internal services, excessive permissions, and business-logic flaws. Low-impact duplicates (reflected XSS with no login, low-value info leaks) often get small or zero payouts.

---

## 3) Improve reconnaissance and mapping

* Inventory all entry points: APIs, mobile backends, graphQL endpoints, admin panels, subdomains, staging/CI endpoints, S3 buckets, dev consoles, client-side JS.
* Use manual review of JS for secrets, dynamic HTTP endpoints, and hidden parameters — automation misses context.
* Build a target-specific checklist: auth flows, password reset, invite flows, file upload, rate limits, 2FA, email flows, third-party integrations.

---

## 4) Master chaining and context

Many high-value bugs require chaining smaller issues (e.g., SSRF -> metadata leak -> token -> RCE). Think in terms of what an attacker could *combine* with the bug. Read API docs, client-side code, and error messages to find chaining opportunities.

---

## 5) Become excellent at reproductions / PoCs

* Provide a **minimal, deterministic PoC** that works for triage. Prefer safe, non-destructive proofs (screenshots, curl commands, request/response dumps, short script).
* Include exact environment (URL, headers, cookies/session, user role) and step-by-step reproduction. Use copy-pastable commands (curl, httpie) and a short script when helpful.
* If timing or sequence matters, include a short screencapture (GIF or MP4) showing the exploit. Video + text together is excellent.

What to include in PoC:

* Full HTTP request and response (raw).
* cURL that reproduces in one command.
* If an interactive session needed, a short Node/Python script.
* Network capture (pcap) for complex flows.

---

## 6) Write succinct, engineer-friendly reports

* Start with a 1–2 line *impact summary* (what an attacker can achieve).
* Follow with *exact* reproduction steps (copy-pastable).
* Attach PoC and logs.
* Suggest a fix and where to add checks (e.g., “add allowlist for redirect hostnames” or “encode username before echo”).
* Use clear severity justification (why it’s high: auth bypass, PII exposure, scale of users affected).

Good report flow: Summary → Impact → Repro steps → PoC → Scope/affected versions → Suggested fix → Attachments.

---

## 7) Be accurate and reasonable with severity and payout asks

* Don’t demand unrealistic sums. Provide CVSS-like reasoning or attack scenarios to support high severity.
* If they disagree with your severity, ask for the technical reason — a polite back-and-forth usually leads to a reassessment or partial payout.

Quick CVSS-style mapping (informal):

* RCE / DB access / ATO across all users → High/Critical.
* Sensitive PII exposure (many users) → High.
* SSRF to internal leading to metadata leak → High.
* Reflected XSS on a low-traffic page → Low/Medium.

---

## 8) Speed & timing matter — but don’t be sloppy

* Send the report through the program’s official channel (HackerOne/Bugcrowd) or security@ before public posting.
* Fast, clear initial reports get acknowledged and move through triage faster.
* If you find something during a busy time (outage/holiday), be extra clear and patient.

---

## 9) Build credibility

* Keep a good track record: polite behavior, follow responsible disclosure, avoid harmful testing.
* Publish write-ups (after coordinated disclosure) of non-sensitive bugs to show your methodology.
* Maintain a public profile (HackerOne, GitHub, blog) describing past valid findings — triagers prefer known, professional hunters.

---

## 10) Use automation smartly but prioritize manual validation

* Automate discovery for scale (subdomain enumeration, endpoints, parameter fuzzing).
* Manually verify and triage every automation hit — many are false positives or not exploitable. Engineering rewards clean, reproducible reports, not noisy lists of unverified findings.

Tools to automate reconnaissance (use responsibly):

* Passive: certstream/CT, sublist3r, assetfinders.
* Active: Burp Suite (intruder, repeater), FFUF, sqlmap (careful), custom scripts.
  Automation is a force multiplier — but manual exploitation is where the bounties live.

---

## 11) Network and learn from the community

* Read program write-ups and public reports to learn patterns and triage expectations.
* Join local/online security meetups, Twitter/X, Discord, or forum channels where hunters share techniques and tools.
* Reproduce interesting public write-ups to learn how the bug was found and reported.

---

## 12) Negotiate & escalate politely

* If you feel your report is undervalued, provide a calm technical justification and examples of impact.
* If a program is unresponsive or refuses to pay for a clear high-impact bug, you can escalate via platform mediation (HackerOne/Bugcrowd) or national CERTs — but keep communications professional.

---

## 13) Checklist before submitting a report

* ☐ Is the issue in-scope?
* ☐ Can I reproduce it with a single PoC?
* ☐ Have I included raw requests and responses?
* ☐ Have I described attacker impact clearly?
* ☐ Have I suggested a remediation?
* ☐ Is my testing non-destructive and legal per policy?
* ☐ Have I provided contact and optional PGP?

---

## Quick sample one-line impact statement (use at top of the report)

“Logged-in users can change another user’s email address without authorization by tampering the `user_id` parameter — leads to account takeover (affects all users).”

---


