---
name: Search and book a hotel
description: Use the Despegar B2B hotels + booking APIs to go from a destination search to a confirmed reservation.
api: openapi/despegar-hotels-openapi.json
operations: [searchAvailability, getHotelAvailability, createPrebook, getPaymentOptions, createBooking, getReservation]
---

# Search and book a hotel (Despegar B2B)

Authenticate every call with the `x-apikey` header (see `authentication/despegar-authentication.yml`). Use the sandbox key against `https://api-dev.despegar.com/v3` while testing.

## Steps

1. `searchAvailability` — search available hotels for the destination and stay dates. Pass `lang` (`pt`|`en`|`es`). Dates earlier than today are rejected.
2. `getHotelAvailability` — fetch live availability and rates for a specific hotel from the search results.
3. `createPrebook` — lock the selected rate. Send the encrypted `choice_id` returned by availability. This binds the quote before purchase (there is no idempotency key; the prebook context is the reservation guard).
4. `getPaymentOptions` — retrieve the payment options available for the prebooked selection.
5. `createBooking` — confirm the reservation using the prebook context and chosen payment.
6. `getReservation` — read back the reservation to confirm final status.

## Rules

- Handle `410 Gone` on prebook by re-running availability to get a fresh `choice_id` (the quote expired).
- Handle `429` with backoff on search.
- `403` means the product is not certified/enabled for your key — confirm hotels is contracted.
