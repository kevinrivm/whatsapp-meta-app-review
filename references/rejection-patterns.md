# Common rejection patterns and fixes

Use this reference when the user already submitted for App Review and needs to understand why Meta rejected the request.

## Pattern: "The screencast does not match the use case details"

Typical cause:

- The app notes describe a full flow, but the video only shows onboarding or only shows a dashboard.

Fix:

- Re-record the screencast so the product behavior and the written notes match exactly.
- Include onboarding, asset reading, sending, and the recipient device.

## Pattern: Incomplete permission proof

Typical cause:

- `whatsapp_business_management` was requested, but the app never visibly loads phone assets or templates.
- `whatsapp_business_messaging` was requested, but the app never shows a real template send.

Fix:

- Make the critical areas real before reapplying.
- Show those areas clearly in the product UI.

## Pattern: Product looks mocked

Typical cause:

- Core screens contain only static data.

Fix:

- Replace the permission-critical sections with real data:
  - connected number,
  - template list,
  - recent message activity,
  - or real send outcome.

## Pattern: Review build breaks during the flow

Typical cause:

- missing environment variables,
- OAuth mismatch,
- failed code exchange,
- number not operational,
- no approved template available.

Fix:

- Make the review build boringly reliable.
- Remove debug UI from the screencast surface after troubleshooting.

## Pattern: Onboarded number stays "pending" and cannot send

Typical cause:

- Embedded Signup completes and the token is exchanged, but the SaaS never
  calls `POST /{phone_number_id}/register`. The number stays `PENDING` in Meta
  and cannot send the approved template the reviewer needs to see.

Why it blocks approval:

- The whole submission depends on showing a real approved template being sent
  from the connected number. A `PENDING` number cannot send, so the screencast
  cannot prove `whatsapp_business_messaging`.

Fix:

- Add the register step to the product so onboarding ends with an operational
  number, not a pending one:

```text
POST https://graph.facebook.com/v21.0/{phone_number_id}/register
Authorization: Bearer {access_token}
Content-Type: application/json

{ "messaging_product": "whatsapp", "pin": "000000" }
```

- Run it right after the code exchange, or as a background worker that
  registers any newly connected number that is still pending.
- Treat `{ "success": true }` and error code `133016` (already registered) as
  success, then mark the number operational locally.
- Re-record the screencast only after the number can actually send, because a
  re-onboarded number gets a new `phone_number_id` that starts pending again.
- See `whatsapp-saas-meta-infra` for the full register endpoint and worker.

## Approval checklist

- Production route exists and loads.
- Embedded Signup completes.
- Connection persists.
- Number appears in app.
- Number is registered and operational (not `PENDING`).
- Template exists and is approved.
- Message is sent from the app.
- Recipient device shows the message.
