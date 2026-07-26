---
name: Search flights and start a booking
description: Use the Despegar B2B flights API to search availability and begin the prebook/booking flow.
api: openapi/despegar-flights-openapi.json
operations: [searchFlightsAvailability, createPrebook, getPaymentOptions, createBooking]
---

# Search flights and start a booking (Despegar B2B)

Authenticate with the `x-apikey` header. Test against `https://api-dev.despegar.com/v3`.

## Steps

1. `searchFlightsAvailability` — search flight availability for the origin/destination and dates. Set `lang` (`pt`|`en`|`es`).
2. `createPrebook` — lock the selected itinerary with the encrypted `choice_id` from the search response.
3. `getPaymentOptions` — retrieve payment options for the prebooked itinerary.
4. `createBooking` — confirm the booking with the prebook context.

## Rules

- `503 FLIGHT_PROVIDER_ERROR` is a downstream airline/provider error — retry or surface to the user; it is not a client fault.
- `410` on prebook/upsell means the fare is gone — re-search.
- Respect `429` rate limiting with backoff.
