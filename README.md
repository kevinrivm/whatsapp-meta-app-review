# whatsapp-meta-app-review

Claude Code / Agent skill to **prepare, debug, and resubmit Meta App Review**
for a WhatsApp SaaS using Embedded Signup, `whatsapp_business_management`, and
`whatsapp_business_messaging`.

Use it when:

- drafting permission usage notes and reviewer-facing copy,
- writing the screencast script,
- reviewing rejection reasons and mapping each one to the missing proof,
- turning a partially working WhatsApp SaaS into an approval-ready product.

## Install

Clone into your skills directory:

```bash
git clone https://github.com/kevinrivm/whatsapp-meta-app-review.git \
  ~/.claude/skills/whatsapp-meta-app-review
```

## Structure

- `SKILL.md` — entry point, workflow, guardrails, and minimum proof standard.
- `references/approved-flow.md` — approved screencast structure and narration.
- `references/submission-copy.md` — permission notes and reviewer language.
- `references/rejection-patterns.md` — common rejection causes and concrete
  fixes, including the "number stays pending and cannot send" pattern.

## Key idea

Approval depends on showing real in-product evidence: the business connects its
own account, the onboarded number is **registered and operational** (not
`PENDING`), the app reads real assets, sends an approved template, and the
recipient device shows the delivered message.

For the implementation side of the `/register` step, see the companion skill
[`whatsapp-saas-meta-infra`](https://github.com/kevinrivm/whatsapp-saas-meta-infra).
