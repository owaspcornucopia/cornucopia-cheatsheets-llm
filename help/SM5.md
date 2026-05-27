[← Back to Cheat Sheet](/README.md)

# SM5 — Predict or Guess Session Identifiers

## Non-Applicable

This threat does not apply to this application.

## Reasoning

The SM5 card describes predicting or guessing session identifiers because they lack sufficient randomness or are not changed periodically.

The tokens in this application are UUID v4 values (128 bits of randomness). Predicting an unknown UUID through randomness analysis is computationally infeasible. While the tokens have other serious problems:

- They never change (covered by SM6, AT6)
- They are exposed in source code (covered by CRJ, AT5)
- They can be intercepted in transit (covered by SM9, CR6)

The specific attack of predicting or guessing a token value through insufficient randomness is not realistic given the UUID format. The actual attack vector is obtaining tokens through other means (source code access, network interception), not guessing them.
