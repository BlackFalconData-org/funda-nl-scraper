# Funda Netherlands Scraper

Extract structured data from [funda.nl](https://funda.nl) — funda.nl — Dutch homes for sale and rent. Every record carries address, asking price and energy label the source publishes. Agent contact details are included where the source lists them. Structured rows, ready to import into a CRM or a spreadsheet.

**[Funda Netherlands Scraper on Apify →](https://apify.com/blackfalcondata/funda-nl-scraper?fpr=1h3gvi)**

---

## 🚀 How to use this actor

> ### 💚 $5 free Apify credits — every month
> No credit card required. No commitment. Cancel anytime.

### 👉 [Sign up free on Apify →](https://console.apify.com/sign-up?fpr=1h3gvi)

1. **Click sign up** — pick GitHub, Google, or email; takes ~30 seconds
2. **Open this actor** — input is pre-filled with a working example
3. **Click Start** — export results as JSON, CSV, or Excel

Your **$5 monthly platform credit** is enough to run this actor right away — and again every month — scraping typically several hundred to several thousand results per run, depending on your input.

## Key features

**Search with filters** — Search by keyword and location. Filter by 🌐 content language, description format, and more.

**Detail enrichment** — Fetch full descriptions, contact information for each listing.

**Incremental mode** — Only get new or changed listings since your last run. Content hash per listing — no duplicates, no re-processing.

**Change classification** — Track unchanged, expired, cross-run duplicate detection across runs. Build audit trails of how listings evolve over time.

**Compact output** — Emit core fields only (AI-agent / MCP-friendly). Keeps response size small for LLM workflows.

**Description truncation** — Cap description length per ${NOUN} to control output size and cost.

**Result cap** — Stop after N listings (up to 1.000). Set to 0 for the full catalog.

**Export anywhere** — Download as JSON, CSV, or Excel. Stream via Apify API, webhooks, or integrations with Make, Zapier, Airbyte, Keboola.

**Structured data** — Every listing returns the same schema with consistent field naming. All fields always present — `null` when unavailable, never omitted.

---

## Use cases

**Data pipeline automation**
Integrate with your ETL pipeline to collect structured listings from funda.nl on a schedule. Export to CSV, JSON, or directly to your database. Use compact mode to control output size.

**Market research**
Monitor listings, track trends, and analyze market dynamics with structured, deduplicated data from funda.nl.

**Change monitoring**
Run daily or hourly in incremental mode to capture only new, updated, or expired ${NOUNS}. Perfect for price-tracking, churn analysis, and alerting pipelines.

**Lead generation**
Extract contact details alongside ${NOUNS} to build targeted outreach lists for sales teams, partnerships, or B2B prospecting.

**AI / LLM training data**
Structured JSON per listing is ready for RAG pipelines, embeddings, and agent workflows. Compact mode trims tokens for LLM context windows.

---

## Quick start

```json
{
  "query": "koop",
  "maxResults": 100,
  "includeDetails": true,
  "descriptionMaxLength": 0,
  "compact": false,
  "descriptionFormat": "all",
  "excludeEmptyFields": false,
  "incrementalMode": false,
  "skipReposts": false,
  "emitUnchanged": false,
  "emitExpired": false,
  "notifyOnlyChanges": false,
  "notificationLimit": 5
}
```

---

## Input parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `query` | string | `"koop"` | What to search. Type a listing type (koop = for sale, huur = for rent, nieuwbouw = new-build, recreatie = recreational, europe = abroad) or a Dutch city name such as Amsterdam. A city on its own searches every Dutch listing type in that city. Use a JSON array for several, e.g. ["huur","Utrecht"]. Required if no startUrls provided. |
| `location` | string | — | Optional. Narrow the results to one city, written as it appears in the listing URL (e.g. "amsterdam", "den-haag"). Leave empty for the whole country. Use a JSON array for several cities, e.g. ["amsterdam","utrecht"]. |
| `maxResults` | integer | `100` | Maximum total results (0 = unlimited). |
| `startUrls` | array | `[]` | Paste raw search URLs from the target site. Each URL becomes its own search task; results are merged and deduped by listing ID across all URLs. When provided, startUrls REPLACE the query/location fields (each supported URL becomes one task; unsupported URLs are skipped, and if none is supported the run stops with an input error). |
| `searchKeywords` | array | — | Only keep listings whose title, description or features mention at least one of these words (not case- or accent-sensitive), e.g. garden, garage. |
| `searchPriceMin` | integer | — | Lowest asking price (sale) or monthly rent (rental). |
| `searchPriceMax` | integer | — | Highest asking price (sale) or monthly rent (rental). |
| `minLivingArea` | integer | — | Smallest living area. |
| `maxLivingArea` | integer | — | Largest living area. |
| `minPlotArea` | integer | — | Smallest plot size. |
| `maxPlotArea` | integer | — | Largest plot size. |
| `minRooms` | integer | — | Minimum number of rooms. |
| `minBedrooms` | integer | — | Minimum number of bedrooms. |
| `objectTypes` | array | — | Keep only these property types. Leave empty for all. |
| `energyLabels` | array | — | Keep only listings with one of these energy labels. Leave empty for all. |
| `constructionTypes` | array | — | Existing homes (resale) or new-build. Leave empty for both. |
| `publishedWithinDays` | integer | — | Only listings published in the last N days. |
| `radiusKm` | integer | — | With a City set, keep only listings within this distance (km) of the city centre. A radius in a pasted search URL (e.g. amsterdam,10km) is used when this is empty. |
| `contentLanguage` | enum | `"nl"` | Language of the listing texts (description, feature labels): Dutch original or English. |
| `includeDetails` | boolean | `true` | Fetch every field on each listing (price breakdown, features, energy label, photos, estate-agent contact). Turn off for a fast address + URL list only. |
| `includeNeighborhoodData` | boolean | `false` | Add funda Market Insights stats for each listing's neighbourhood: inhabitants, share of families with children and average asking price per m². One extra request per neighbourhood. Needs Include Full Details. |
| `enrichEmails` | boolean | `false` | Add a contactEmail per listing: the estate agent's email from funda, or, when funda lists none, one found on the agent's own website (homepage and /contact). contactEmailSource says which. Best-effort. Needs Include Full Details. |
| `descriptionMaxLength` | integer | `0` | Truncate description to N chars. 0 = no truncation. |
| `compact` | boolean | `false` | Core fields only (for AI-agent/MCP workflows). |
| `descriptionFormat` | enum | `"all"` | Pick a single description representation. `all` keeps every variant; `text` / `html` / `markdown` drop the others. |
| `excludeEmptyFields` | boolean | `false` | Drop null, empty-string, and empty-array fields from each record before push. Smaller payloads for AI agents and dashboards. |
| `incrementalMode` | boolean | `false` | Compare against previous run state. stateKey is optional — defaults to a value derived from search inputs (listing type, cities, startUrls) so different filter sets never share state. |
| `stateKey` | string | — | Optional. Stable identifier for the tracked search universe. Leave empty to auto-generate from search inputs. |
| `skipReposts` | boolean | `false` | When incremental, skip listings whose content matches an expired listing from a prior run (cross-run duplicate detection). |
| `emitUnchanged` | boolean | `false` | When incremental mode is on, also emit listings whose content has not changed since the last run. |
| `emitExpired` | boolean | `false` | When incremental mode is on, also emit listings that were seen before but are no longer found. |
| `notifyOnlyChanges` | boolean | `false` | When Incremental Mode is on, only send notifications for NEW and UPDATED listings. Has no effect outside incremental mode. |
| `notificationLimit` | integer | `5` | Maximum number of listings included in each notification message (1–20). |
| `telegramToken` | string | — | Telegram bot token (from @BotFather). Required for Telegram notifications. |
| `telegramChatId` | string | — | Telegram chat or channel ID (e.g. "-100123456789"). Required when telegramToken is set. |
| `discordWebhookUrl` | string | — | Discord incoming webhook URL. Server Settings → Integrations → Webhooks → New Webhook. |
| `slackWebhookUrl` | string | — | Slack incoming webhook URL. api.slack.com/messaging/webhooks. |
| `whatsappPhoneNumberId` | string | — | Your WhatsApp Business phone-number ID (numeric, from Meta dashboard). Required when whatsappAccessToken is set. |
| `whatsappAccessToken` | string | — | WhatsApp Cloud API permanent access token (System User token from Meta Business). Recipient must have messaged the business number within the last 24h (service-conversation window — free since Nov 2024). |
| `whatsappTo` | string | — | Recipient phone in E.164 format without + (e.g. "436641234567"). Recipient must have messaged your business number within last 24h. |
| `webhookUrl` | string | — | Receives a JSON POST with {metadata, items} after each run. Universal escape hatch for n8n / Make / Zapier / custom backends. |
| `webhookHeaders` | object | — | Optional JSON object of custom headers (e.g. {"Authorization":"Bearer ..."}). |
| `appConnector` | string | — | Optional. Pick a connected app under Settings → API & Integrations to receive your results (including any contact details). Best-effort across MCP connectors as Apify expands its catalog. |
| `mcpIssueTeam` | string | — | Only when the connected app is an issue tracker: the team (name or ID) the summary issue is created under, if that app requires one. |

---

## Output fields

Every listing returns the same 117-field schema. Missing values are `null` — never omitted.

- `listingId`
- `tinyId`
- `url`
- `portalUrl`
- `listingType`
- `category`
- `offeringType`
- `alsoForRent`
- `status`
- `statusText`
- `isSoldOrRented`
- `labels`
- `title`
- `price`
- `priceText`
- `priceCondition`
- `sellingPrice`
- `rentPrice`
- `currency`
- `pricePerM2`
- `isAuction`
- `vveContributionMonthly`
- `acceptance`
- `listedSince`
- `publicationDate`
- `street`
- `houseNumber`
- `houseNumberSuffix`
- `postcode`
- `city`
- `citySlug`
- `municipality`
- `province`
- `neighbourhood`
- `neighbourhoodId`
- `country`
- `latitude`
- `longitude`
- `lat`
- `lon`
- `googleMapsUrl`
- `objectType`
- `kindOfHouse`
- `constructionType`
- `constructionYear`
- `roofType`
- `livingArea`
- `surfaceM2`
- `plotArea`
- `volume`
- `exteriorSpaceArea`
- `externalStorageArea`
- `rooms`
- `bedrooms`
- `bathrooms`
- `toilets`
- `floors`
- `locatedOn`
- `energyLabel`
- `insulation`
- `heating`
- `hotWater`
- `boiler`
- `balconyOrTerrace`
- `surroundings`
- `parking`
- `storage`
- `ownership`
- `groundLease`
- `cadastralParcels`
- `hasGarden`
- `hasBalcony`
- `hasRoofTerrace`
- `hasSolarPanels`
- `hasHeatPump`
- `parkingOnOwnProperty`
- `parkingOnEnclosedProperty`
- `isFixerUpper`
- `isMonument`
- `hasOpenHouse`
- `description`
- `highlights`
- `mainImage`
- `imageUrl`
- `images`
- `imageCount`
- `floorPlans`
- `videos`
- `photos360`
- `brochureUrl`
- `makelaarId`
- `makelaarName`
- `makelaarAssociation`
- `makelaarPhone`
- `makelaarEmail`
- `makelaarWebsite`
- `makelaarAddress`
- `makelaarCity`
- `makelaarUrl`
- `makelaarRating`
- `makelaarReviewCount`
- `neighbourhoodInhabitants`
- `neighbourhoodFamiliesWithChildrenPct`
- `neighbourhoodAvgAskingPricePerM2`
- `neighbourhoodAvgAskingPrice`
- `neighbourhoodAvgDaysOnMarket`
- `features`
- `language`
- `searchInput`
- `sitemapLastModified`
- `searchQuery`
- `contentQuality`
- `detailFetched`
- `scrapedAt`
- `source`
- `contentHash`
- `changeType`

---

## Sample output

One object per listing. Here is a real example from a production run:

```json
{
  "listingId": 8205309,
  "tinyId": "17387035",
  "url": "https://www.funda.nl/detail/koop/amsterdam/appartement-het-hoogt-504/17387035/",
  "portalUrl": "https://www.funda.nl/detail/koop/amsterdam/appartement-het-hoogt-504/80014308/",
  "listingType": "apartment",
  "category": "residential",
  "offeringType": "sale",
  "alsoForRent": false,
  "status": "beschikbaar",
  "statusText": "Beschikbaar",
  "isSoldOrRented": false,
  "labels": [
    "Nieuw"
  ]
}
```

*Truncated — full records contain 117 fields. See Output fields for the complete schema.*

**[Try Funda Netherlands Scraper now — $5 free credit, no credit card →](https://apify.com/blackfalcondata/funda-nl-scraper?fpr=1h3gvi)**

---

## Pricing

Pay only for what you extract. No subscription required — Apify's free $5 credit covers thousands of results.

| Event | Price (USD) |
| --- | --- |
| Actor Start | $0.01 |
| Result | $0.000675 |

See the [actor on Apify](https://apify.com/blackfalcondata/funda-nl-scraper?fpr=1h3gvi) for current pricing.

---

## FAQ

**How do I scrape funda.nl?**
Use this actor on Apify to extract structured data from funda.nl. Configure your search query and filters in the input, then click Start — no coding required.

**How do I get funda.nl data as JSON, CSV, or Excel?**
The actor writes each listing to Apify's dataset. Download as JSON, CSV, or Excel from the Console, stream via the API, or push to Make, Zapier, Airbyte, or Keboola.

**Is it legal to scrape funda.nl?**
Web scraping of publicly available data is generally legal. This actor only accesses publicly visible information. Always check funda.nl's terms of service for your specific use case.

**How much does it cost?**
Pay-per-event pricing — you only pay for ${NOUNS} extracted. Apify's free $5 credit is enough to run thousands of results before you pay anything.

**How does incremental mode work?**
Each listing gets a content hash. On subsequent runs, only new or changed listings are emitted — saving time, compute, and storage. Expired listings can be tracked separately.

**Do I need an API key or credentials?**
No. Just sign up for Apify, paste your input, and click Start. No credit card required.

---

## Related products by Black Falcon Data

[Browse all Black Falcon Data actors →](https://apify.com/blackfalcondata?fpr=1h3gvi)

---

## Getting started with Apify

New to Apify? [Create a free account with $5 credit](https://console.apify.com/sign-up?fpr=1h3gvi) — no credit card required.

1. Sign up — $5 platform credit included
2. Open [Funda Netherlands Scraper](https://apify.com/blackfalcondata/funda-nl-scraper?fpr=1h3gvi) and configure your input
3. Click **Start** — export results as JSON, CSV, or Excel

Need more later? [See Apify pricing](https://apify.com/pricing?fpr=1h3gvi).

---

## About Black Falcon Data

Black Falcon Data builds production-grade web scrapers for job boards and marketplace data. Browse our full actor catalog at [www.blackfalcondata.com](https://www.blackfalcondata.com).

---

*Last updated: 2026 10*
