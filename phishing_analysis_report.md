# Phishing Email Analysis Report

**Sample:** "🎁 Exclusive: Grab up to $8250 at SpinReelz now"
**Source:** public phishing sample collection (saved as `.eml`, opened as text only; no links or attachments were opened)
**Tools:** Notepad (raw header review), Google Admin Toolbox Messageheader
**Date sent:** 8 Jul 2026, 23:13 UTC

## Verdict
**Malicious bulk spam / malvertising (high confidence).** The email impersonates a casino brand, fails sender authentication, hides its real link destinations behind tracking redirects, and uses filter-evasion tricks. It does not contain a fake login page, so it is closer to unsolicited promotional spam than credential-stealing phishing, but it uses the same core tactics.

## Indicators found

### 1. Sender / spoofing
| Indicator | Evidence |
|---|---|
| Display name does not match the address | Display name "SpinReelz Club 💸💸", actual address `parker.rivera@gbnfrostbornwarriors02.onmicrosoft.com` |
| Free trial domain used | `onmicrosoft.com` is a Microsoft 365 tenant domain anyone can register; it has no link to the SpinReelz brand |
| Name and address unrelated | A person's name ("parker.rivera") sending as a casino club |

### 2. Header analysis
| Check | Result | Meaning |
|---|---|---|
| SPF | **FAIL** | Sending server `74.125.224.52` (a Google mail server) is not authorized for the sender's domain |
| DKIM | pass, but for `gappssmtp.com` | The signature belongs to a different domain than the From address, so the pass does not prove the sender's identity |
| DMARC | **none** | The domain publishes no policy, so receivers have nothing telling them to reject failures |
| Microsoft spam score | `SCL: 8`, `X-SID-Result: FAIL`, `RF:JunkEmail` | Microsoft classified it as high-confidence junk |
| Origin | Submitted via the Gmail API (`gmailapi.google.com ... HTTPREST`) | Sent by automation, not typed in a mail client |
| Delivery speed | Delivered after 2 seconds | Consistent with bulk sending |
| Recipient | `phishing@pot`, no personal name | Generic mass mailing |

Note: Google Admin Toolbox shows the SPF IP as "Unknown" because of how the hops are parsed; the raw header (`Received-SPF: Fail ... client-ip=74.125.224.52`) confirms the IP.

### 3. Links and attachments
| Indicator | Evidence |
|---|---|
| All links go to one unrelated domain | Every button points to `woondsplay.com/...`, never to a SpinReelz site |
| Tracking redirect URLs | Long encoded strings in the path; the final destination is hidden |
| Tracking pixel | 1x1 hidden image at `woondsplay.com/track/...` reports when the email is opened |
| Images hosted elsewhere | Logo and banner loaded from `imgprom.com`; unsubscribe icon from `imgur.com` |
| Weak unsubscribe | Image link at 50% opacity over plain `http://` (not HTTPS) |
| Attachments | None |

### 4. Filter-evasion techniques
- Hidden `display:none` block of random characters at the top of the HTML
- Zero-width characters in the preview text
- Plain-text and HTML parts built from the same template
- Malformed HTML, with extra content after `</html>`

### 5. Language and social engineering
- Too-good-to-be-true bait: "425% up to C$8250 + 525 spins"
- Exclusivity and urgency cues: "Exclusive", "New Players Only", "the next big line is one pull away"
- Subject shows `$8250` while the body says `C$8250` (inconsistent)
- Minor grammar slips ("whether you or someone you know choose to gamble")
- A responsible-gambling paragraph added to look legitimate

## Summary of phishing traits
1. Spoofed or misleading sender identity (display name vs free-tenant address)
2. Failed authentication (SPF fail, DKIM for a different domain, no DMARC)
3. Hidden link destinations via tracking redirects on an unrelated domain
4. Open-tracking pixel
5. Filter-evasion content in the HTML
6. Enticing, urgency-driven offer
7. Landed in the Junk folder with a high spam score

## Recommended actions
- Do not click any link or image; do not reply or "unsubscribe"
- Report as phishing/spam in the mail client and delete
- Block the sender domain and, in an organization, report it to the security team
- Never enter credentials or payment details on a site reached from an unsolicited email
