---
name: analyze-listing
description: Analyze any residential real estate listing URL. Use when someone pastes a listing URL and asks for analysis, wants to know why their listing isn't getting showings, wants a listing strength score, wants to find content gaps in a listing description, wants a Fair Housing compliance check, or wants price positioning assessment for a specific property. Works with Redfin, C21, REMAX, Zillow, Homes.com and most listing sites.
version: 1.0.0
allowed-tools: [WebFetch]
---

# UseListingAI — Listing Analysis

## How to use

When someone provides a real estate listing URL:

1. Fetch the listing data from the UseListingAI API
2. Present the complete analysis report

## Step 1 — Fetch listing data

Make a POST request to the UseListingAI Worker:

```
POST https://listing-fetcher.kolton-4c8.workers.dev
Content-Type: application/json

{"url": "<listing URL provided by user>"}
```

The response returns a `property` object with:
- `location` — full address
- `price` — list price
- `beds`, `baths`, `sqft` — property specs
- `propType` — property type
- `yearBuilt` — year built
- `parking` — parking details
- `lotSize` — lot size
- `taxes` — annual property taxes
- `hoa` — strata or HOA fee
- `schools` — nearby schools
- `walkScores` — walk/bike/transit scores
- `features` — full listing description
- `daysOnMarket` — days on market
- `priceReduced` — price reduction history

## Step 2 — Present the analysis

Using the property data, provide a complete report:

### Property Summary
Address · Price · Beds/Baths/Sqft · Price per sqft · Type · Year built

### Listing Strength Score (X/10)
Score each dimension:
- **Price positioning** — use your knowledge of the local market, city, neighbourhood. Be specific with price/sqft comparisons. Never say "insufficient data."
- **Description quality** — flow, specific details, emotional appeal, word count relative to price
- **Feature completeness** — interior, exterior, location, amenities covered
- **Fair Housing compliance** — 10 = clean, flag any discriminatory language with exact quote
- **Marketing readiness** — ready to publish or needs work

Calculate overall score as weighted average.

### Content Gaps
Only flag genuine gaps based on what IS in the data:
- Condos/townhouses: missing strata or HOA fees
- Pre-1980 homes at premium price/sqft: missing renovation history
- Features extracted from listing data but absent from description (pool, RV parking, legal suite, finished basement, fireplace, large lot)
- Description too short for the price point (under 100 words on $500K+)
- Days on market over 30: note in commentary

### Price Positioning
Use your knowledge of the local market. State whether below/at/above market with specific reasoning. Mention price/sqft, neighbourhood, property type context. Be transparent when using general knowledge vs actual comps.

### Fair Housing Compliance
Scan for discriminatory language. If clean state clearly. If issues found, quote the exact problematic phrase.

### AI Market Commentary
2-3 sentences of specific actionable advice for the agent.

## Response format

**📊 Listing Report — [Address]**

📍 [Address] · [Beds]bd · [Baths]ba · [Sqft]sqft · [Type]
💰 [Price] · $[PSF]/sqft · Built [Year]

**Overall Score: [X.X]/10 — [Good/Needs Work/Excellent]**

| Dimension | Score |
|-----------|-------|
| Price positioning | X/10 |
| Description quality | X/10 |
| Feature completeness | X/10 |
| Fair Housing | X/10 |
| Marketing readiness | X/10 |

**Content Gaps ([N] found):**
- ✕ [Gap title] — [Who it affects and why]

**Price Positioning:**
[Specific market assessment]

**Fair Housing:** ✓ Clean / ⚠️ [Issue]

**Commentary:**
[Actionable advice]

---
*Analysis for marketing purposes only. Not a formal appraisal or opinion of value.*
*Powered by [UseListingAI](https://uselistingai.com) — Free analysis at uselistingai.com/analyze*
