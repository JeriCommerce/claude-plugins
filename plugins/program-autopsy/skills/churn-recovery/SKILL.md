---
name: churn-recovery
description: >
  Draft a personalized recovery message for a churned JeriCommerce merchant based
  on autopsy findings. Use after running a program autopsy when the user wants to
  reach out, contact the merchant, write a recovery email, send a follow-up, or
  try to win them back. Trigger phrases include "contactar al cliente", "escribir
  email de recuperacion", "reach out to the merchant", "win them back", "recovery
  email", "follow up after churn", "contactar al merchant", "mandar mensaje",
  or any reference to contacting a merchant after investigating their program.
version: 202604.08.0
---

# Churn Recovery

Draft a personalized recovery message to a churned JeriCommerce merchant, using evidence from the program autopsy to address their specific pain points.

## Required Context

This skill works best after a program autopsy has been run. If no autopsy data is available in the conversation, ask the user for the program slug and run the autopsy first (read `${CLAUDE_PLUGIN_ROOT}/skills/program-autopsy/SKILL.md`).

From the autopsy, extract:
- **Shop name / domain**
- **Session duration** — how long they explored before churning
- **Churn signals** — the specific reasons they left
- **Configuration state** — what they set up vs. what they didn't reach
- **Errors encountered** — backend failures during their session

## Required Tools

- `mcp__claude_ai_Gmail__gmail_create_draft` — Create a draft email in Gmail (never send directly)

If Gmail is unavailable, output the message as text for the user to copy.

## Recovery Strategy by Churn Signal

Choose the primary angle based on the highest-confidence churn signal:

| Churn Signal | Recovery Angle | What to Offer |
|-------------|----------------|---------------|
| Price sensitivity | Value reframe | Highlight free tier capabilities, ROI comparison, offer extended trial |
| Broken first impression | Acknowledge + fix | Apologize for the bad experience, confirm it's been fixed, invite retry |
| Generic card appearance | Show the potential | Share examples of well-branded cards in their vertical, offer design help |
| Quick bounce | Guided onboarding | Offer a personal walkthrough or setup session |
| No real customer testing | Activation help | Share quick-win enrollment strategies for their vertical |
| Backend errors during onboarding | Acknowledge + fix | Apologize for technical issues, confirm resolution, offer setup assistance |
| No program integrations | Setup assistance | Offer to help configure integrations during a call |
| Exploration without commitment | Remove friction | Address what held them back, simplify next step |

## Writing Rules

### Tone
- Warm, peer-level, not salesy — like a colleague who genuinely wants to help
- Acknowledge what happened without being defensive
- Show you understand their specific situation (use autopsy details)
- Write in the merchant's language (detect from shop domain / PostHog data, default to Spanish for .es domains, English otherwise)

### Structure
1. **Opening** (1 sentence): Reference something specific about their experience — not "we noticed you left" but a detail that shows you understand
2. **Acknowledgment** (1-2 sentences): Address the likely pain point directly. If there was a bug or error, own it
3. **Value bridge** (1-2 sentences): Connect to what they were trying to achieve (based on what they configured)
4. **Offer** (1 sentence): One specific, low-friction next step
5. **Close**: Casual sign-off, no pressure

### Constraints
- Under 120 words total
- Never mention competitors by name — use "your current loyalty provider" or similar
- No generic "we'd love to have you back" language
- No discount offers unless the user explicitly requests it
- One CTA only — and make it easy (reply, quick call, retry link)
- Never send automatically — always create as draft or show as text
- Banned words: seamless, innovative, leverage, revolutionary, cutting-edge, game-changing

## Output Format

Present the message with:

1. **Subject line** (2-3 options, short and internal-looking — e.g., "loyalty setup", "quick question", "your wallet card")
2. **Email body**
3. **Sending notes**: Best time to send, any personalization the user should add, and which email address to send from
4. **Alternative channel suggestion**: If email feels too formal, suggest a shorter version for Intercom/chat

## Multi-Message Sequence

If the user asks for a sequence (not just a single message):

- **Message 1** (Day 0): Address the primary churn signal
- **Message 2** (Day 3-5): Share a relevant success story or resource from their vertical
- **Message 3** (Day 10-14): Final, ultra-short check-in with a different angle

Each message must add something new. Never "just checking in."

## Example Flow

Given an autopsy showing:
- Shop: coffee-house, 31 min session
- Churn signals: price sensitivity (High), generic card appearance (Medium)
- Configured: wallet design (3 saves), earning flows
- Untouched: referrals, push notifications

The recovery email might lead with the value angle (addressing price), acknowledge the branding issue as fixed, and highlight that their wallet design work is saved and ready.
