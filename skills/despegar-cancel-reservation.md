---
name: Cancel a reservation
description: Use the Despegar B2B aftersales API to quote and execute a reservation cancellation.
api: openapi/despegar-aftersales-openapi.json
operations: [getReservationStatus, getCancelAllowance, createCancelQuote, cancelReservation]
---

# Cancel a reservation (Despegar B2B aftersales)

Authenticate with the `x-apikey` header.

## Steps

1. `getReservationStatus` — confirm the reservation exists and read its current status.
2. `getCancelAllowance` — check whether the reservation is eligible for cancellation and under what terms.
3. `createCancelQuote` — obtain a cancellation quote (penalties/refund amount).
4. `cancelReservation` — execute the cancellation once the quote is accepted.

## Rules

- Always quote before cancelling — penalties depend on fare/rate rules returned by `getCancelAllowance`.
- `404` means the reservation id is unknown for your key.
- `400` on the quote usually means the reservation is outside the cancellable window.
