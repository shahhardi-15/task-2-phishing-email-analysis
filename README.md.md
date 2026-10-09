# Task 2: Analyze a Phishing Email Sample

Elevate Labs Cyber Security Internship, Task 2.

## Objective
Identify phishing characteristics in a suspicious email sample and summarize the indicators found.

## What I did
1. Obtained a real phishing/spam sample from a public sample collection, saved as an `.eml` file.
2. Opened it only as text in Notepad (never in a mail client) and did not click any link.
3. Reviewed the sender address and the raw headers.
4. Ran the header through **Google Admin Toolbox Messageheader** and captured the result.
5. Checked the HTML body for links, hidden content, tracking and language.
6. Wrote up the findings in the report.

## Key findings
- **Sender spoofing:** display name "SpinReelz Club" but the address is a free `onmicrosoft.com` trial domain.
- **SPF failed**, DKIM passed for an unrelated domain, DMARC none.
- Microsoft marked it as junk (`SCL: 8`).
- All links go to an unrelated tracking domain; a hidden 1x1 tracking pixel is included.
- Hidden gibberish and zero-width characters are used to evade spam filters.
- Too-good-to-be-true offer ("425% up to C$8250 + 525 spins") with exclusivity and urgency cues.

Verdict: malicious bulk spam / malvertising (uses the core phishing tactics but has no fake login page).

## Files
- `phishing_analysis_report.md`: full report of phishing indicators
- `evidence/`: the email sample (text) and header analyzer screenshots

## Safety note
The sample was handled as plain text only. No links were opened and no attachments were run.
