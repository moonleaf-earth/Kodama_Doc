# Stripe — Test Card Numbers

Use these in **Stripe test mode** (`pk_test_` / `sk_test_` keys). Real cards are rejected in test mode.

## Successful Payments
| Number | Brand | Notes |
|--------|-------|-------|
| `4242 4242 4242 4242` | Visa | Always succeeds |
| `5555 5555 5555 4444` | Mastercard | Always succeeds |
| `3782 822463 10005` | Amex | Always succeeds |

## Declined / Error Cards
| Number | Result |
|--------|--------|
| `4000 0000 0000 0002` | Card declined |
| `4000 0000 0000 9995` | Insufficient funds |
| `4000 0000 0000 0069` | Expired card |
| `4000 0000 0000 0127` | Incorrect CVC |

## 3D Secure
| Number | Result |
|--------|--------|
| `4000 0025 0000 3155` | Requires authentication (succeeds) |
| `4000 0000 0000 3220` | Requires authentication (fails) |

## Other Details
- **Expiry:** Any future date (e.g. `12/34`)
- **CVC:** Any 3 digits (4 for Amex)
- **ZIP:** Any valid format

Ref: https://docs.stripe.com/testing#cards
