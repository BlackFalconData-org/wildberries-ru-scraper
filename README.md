# Wildberries.ru Product Scraper [Just 💰$0.23]

Extract structured data from [Wildberries.ru](https://Wildberries.ru) — Wildberries.ru — product search results by keyword or by search-page URL, with price and sale price in RUB, rating, review count and seller for every product. Optional detail enrichment adds full specs and photos plus the complete price history of each product.

**[Wildberries.ru Product Scraper [Just 💰$0.23] on Apify →](https://apify.com/blackfalcondata/wildberries-ru-scraper?fpr=1h3gvi)**

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

**Search with filters** — Search by keyword and location. Filter by description format, ↕️ sort order, 📈 monitor output, and more.

**Multiple input modes** — products (default) or reviews - one row per review or sellers - one row per seller or niche - one market summary row per keyword or category or monitor - search rank and price tracking between runs or resale - second-hand (c2c) listings. Switch modes without re-scraping.

**Detail enrichment** — Fetch full descriptions, structured metadata for each product.

**Incremental mode** — Only get new or changed products since your last run. Content hash per product — no duplicates, no re-processing.

**Change classification** — Track unchanged, expired, cross-run duplicate detection across runs. Build audit trails of how products evolve over time.

**Compact output** — Emit core fields only (AI-agent / MCP-friendly). Keeps response size small for LLM workflows.

**Description truncation** — Cap description length per ${NOUN} to control output size and cost.

**Result cap** — Stop after N products (up to 10.000). Set to 0 for the full catalog.

**Export anywhere** — Download as JSON, CSV, or Excel. Stream via Apify API, webhooks, or integrations with Make, Zapier, Airbyte, Keboola.

**Structured data** — Every product returns the same schema with consistent field naming. All fields always present — `null` when unavailable, never omitted.

---

## Use cases

**Data pipeline automation**
Integrate with your ETL pipeline to collect structured products from Wildberries.ru on a schedule. Export to CSV, JSON, or directly to your database. Use compact mode to control output size.

**Market research**
Monitor products, track trends, and analyze market dynamics with structured, deduplicated data from Wildberries.ru.

**Change monitoring**
Run daily or hourly in incremental mode to capture only new, updated, or expired ${NOUNS}. Perfect for price-tracking, churn analysis, and alerting pipelines.

**AI / LLM training data**
Structured JSON per product is ready for RAG pipelines, embeddings, and agent workflows. Compact mode trims tokens for LLM context windows.

---

## Quick start

```json
{
  "query": "платье",
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
| `query` | string | — | Product search keywords, as you would type them on the site (Russian works best, e.g. "платье", "кроссовки nike", "iphone 15"). Use a JSON array for several searches in one run, e.g. ["платье","кроссовки"]. Required if no startUrls provided. |
| `location` | string | — | Optional delivery-region id, which decides the prices, stock and delivery times you see. Leave empty for Moscow (-1257786). Other examples: -1123300 (Saint Petersburg), 12358499 (Kazan), -2133463 (Yekaterinburg). Use a JSON array to compare several regions in one run. |
| `maxResults` | integer | `100` | Maximum total results (0 = unlimited). |
| `startUrls` | array | `[]` | Paste raw search URLs from the target site. Each URL becomes its own search task; results are merged and deduped by product ID across all URLs. When provided, startUrls REPLACE the query/location fields (each supported URL becomes one task; unsupported URLs are skipped, and if none is supported the run stops with an input error). |
| `includeDetails` | boolean | `true` | Fetch each product's full card: description, characteristics, pack contents, image list, seller legal data and recorded price history. Turn off for a faster, search-only run (price, stock, rating, seller, rank). |
| `descriptionMaxLength` | integer | `0` | Truncate description to N chars. 0 = no truncation. |
| `compact` | boolean | `false` | Core fields only (for AI-agent/MCP workflows). |
| `descriptionFormat` | enum | `"all"` | Pick a single description representation. `all` keeps every variant; `text` / `html` / `markdown` drop the others. |
| `excludeEmptyFields` | boolean | `false` | Drop null, empty-string, and empty-array fields from each record before push. Smaller payloads for AI agents and dashboards. |
| `mode` | enum | `"products"` | Products (default): one row per product. Reviews: one row per customer review, instead of product rows. Sellers: one row per distinct seller of the products found, instead of product rows. The product search still runs first, and Max Results still limits how many PRODUCTS are read (not how many review or seller rows come out). In Sellers mode the full product details are always loaded, because the seller data comes from them. Niche: one summary row per keyword/category (seller concentration, price spread, stock) from the search results. Monitor: where each product ranks for a keyword now and what changed since the last run (needs the same Monitor state name on every run). Resale: Wildberries Resale (second-hand) listings, optionally only for the conditions chosen below. |
| `scrapeReviews` | boolean | `false` | Products mode: add a reviewAggregates summary (average rating, review counts, 1-5 star distribution) and a reviews list to every product row. In Reviews mode reviews are always collected, so this switch has no effect there. Colour variants of one product share their reviews. |
| `maxReviewsPerProduct` | integer | `20` | Applies to the reviews list on product rows and to the review rows in Reviews mode. 0 = every review the site serves for the product, which is at most its roughly 1,000 most recent ones. Newest reviews come first. Filters below are applied before this limit. |
| `reviewsWithTextOnly` | boolean | `false` | Keep only reviews that have a written comment, pros or cons (skips star-only ratings). |
| `reviewsWithPhotoOnly` | boolean | `false` | Keep only reviews that include at least one customer photo. |
| `maxQuestionsPerProduct` | integer | `0` | Products mode: add the newest customer questions and the seller answers (questions list) plus the total number of questions the product has (questionsCount) to every product row. 0 = off (default). Colour variants of one product share their questions. |
| `incrementalMode` | boolean | `false` | Compare against previous run state. stateKey is optional — defaults to a value derived from search inputs (queries, delivery regions, startUrls) so different searches never share state. |
| `stateKey` | string | — | Optional. Stable identifier for the tracked search universe. Leave empty to auto-generate from search inputs. |
| `skipReposts` | boolean | `false` | When incremental, skip products whose content matches an expired product from a prior run (cross-run duplicate detection). |
| `emitUnchanged` | boolean | `false` | When incremental mode is on, also emit products whose content has not changed since the last run. |
| `emitExpired` | boolean | `false` | When incremental mode is on, also emit products that were seen before but are no longer found. |
| `notifyOnlyChanges` | boolean | `false` | When Incremental Mode is on, only send notifications for NEW and UPDATED products. Has no effect outside incremental mode. |
| `notificationLimit` | integer | `5` | Maximum number of products included in each notification message (1–20). |
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
| `productIds` | array | `[]` | Wildberries article numbers (nmId) to read directly, without a search. Accepts a list, or a comma/space separated text. Ids that do not exist are skipped. Counts as input on its own: queries or start URLs are not needed. |
| `sellerIds` | array | `[]` | Wildberries seller ids to read the catalogue of. A seller id that does not exist produces one row with status not_found and the run still succeeds. Counts as input on its own. |
| `sort` | enum | — | Order of the results for searches, categories, brands, sellers and resale. Empty = the site default (popular). Monitor mode ranks follow this order. |
| `lowPrice` | integer | — | Keep only products at or above this sale price in roubles. Empty = no limit. |
| `topPrice` | integer | — | Keep only products at or below this sale price in roubles. Empty = no limit. |
| `discount` | integer | — | Keep only products discounted by at least this percentage. Empty = no limit. |
| `inStockOnly` | boolean | `false` | Keep only products that are in stock. |
| `minRating` | number | — | Products, niche, monitor, resale and sellers modes: drop products rated below this (0-5). Reviews mode: keep only reviews with at least this star rating (1-5) instead. Empty = no limit. |
| `flagUnderpriced` | boolean | `false` | Products mode: add isUnderpriced and underpricedDeviationPct to each product row. Niche mode: add underpricedCount, underpricedThresholdPrice and underpricedThresholdPct to the niche row. A listing is underpriced when its price is below the baseline x (1 - threshold / 100). Without a reference price the baseline is the median: in niche mode the median of the analysed products, in products mode the median of the first results page of each search/target (so the flag is judged against what that search shows, not the whole result set). |
| `referencePrice` | integer | — | Used with 'Flag underpriced listings': listings below reference price x (1 - threshold / 100) are flagged. Leave empty to compare with the median of the results instead. |
| `underpriceThresholdPct` | integer | `50` | How far below the baseline (reference price or median) a listing must be to count as underpriced. 50 = more than half price. |
| `maxSellersPerSearch` | integer | `25` | Sellers mode: how many distinct sellers to profile from each search query, category or brand page. Each seller row gets catalogue statistics (see 'Products per seller'). |
| `maxProductsPerSeller` | integer | `200` | Sellers mode: how many of a seller's catalogue products to scan for the statistics (price min/max/average/median, distinct brands, average rating, total reviews, in-stock share). 100 products per request, so higher values cost more requests. |
| `brand` | string | — | Keep only products whose brand name contains this text (not case sensitive). Empty = all brands. |
| `maxRating` | integer | — | Reviews mode and review lists: keep only reviews with at most this star rating (1-5), e.g. 2 for complaints. Empty = all reviews. |
| `dateFrom` | string | — | Keep only reviews written on or after this date (YYYY-MM-DD). Empty = no limit. |
| `dateTo` | string | — | Keep only reviews written on or before this date (YYYY-MM-DD). Empty = no limit. |
| `maxProductsPerNiche` | integer | `300` | Niche mode: how many products per keyword or category are analysed (100 per page). More products give a more accurate picture and take longer. |
| `nicheTopN` | integer | `5` | Niche mode: how many of the largest sellers and brands are listed in each summary row. |
| `monitorStateName` | string | `"wb-monitor-default"` | Monitor mode: name of the stored snapshot the run compares against and then updates. Use the same name on every scheduled run of one watch list, and a different name for a different one. Lowercase letters, digits and hyphens. |
| `monitorOutput` | enum | `"snapshot_with_changes"` | Snapshot: current rank and price only. Snapshot with changes: also the previous values and what changed. Changes only: only products that moved, changed price or stock, appeared or disappeared. |
| `maxRankPages` | integer | `5` | Monitor mode: how many result pages (100 products each) are read per keyword to find the rank. A product beyond these pages is reported as missing. |
| `priceChangeThresholdPct` | number | — | Monitor mode: a price move smaller than this percentage is not reported as a change. Empty = any change counts. |
| `targetNmIds` | array | `[]` | Monitor mode: track only these article numbers. A watched product that is not found on the scanned pages is still reported, with isMissing true. Empty = track every product found. |
| `resaleConditions` | array | `[]` | Resale mode: keep only listings in these conditions (new, perfect, good). Empty = all conditions. |

---

## Output fields

Every product returns the same 75-field schema. Missing values are `null` — never omitted.

- `listingId`
- `type`
- `id`
- `nmId`
- `imtId`
- `title`
- `url`
- `portalUrl`
- `brand`
- `brandId`
- `category`
- `parentCategory`
- `subjectId`
- `subjectParentId`
- `entity`
- `price`
- `salePrice`
- `discountPct`
- `currency`
- `priceHistory`
- `lastRecordedPriceRub`
- `lastRecordedPriceDate`
- `promotions`
- `rating`
- `starRating`
- `reviewCount`
- `reviewRewardPoints`
- `inStock`
- `stockQuantity`
- `deliveryHours`
- `estimatedDeliveryDate`
- `isNew`
- `supplier`
- `supplierId`
- `supplierRating`
- `sellerType`
- `sellerTrademark`
- `sellerLegalName`
- `sellerTaxId`
- `sellerRegistrationNumber`
- `sellerIsIndividual`
- `sellerUrl`
- `sellerRegisteredAt`
- `sellerSalesCount`
- `sellerReviewCount`
- `colors`
- `colorName`
- `variantIds`
- `sizes`
- `vendorCode`
- `description`
- `characteristics`
- `characteristicGroups`
- `compositions`
- `packageContents`
- `image`
- `images`
- `imageCount`
- `photoCount`
- `hasVideo`
- `listedAt`
- `createdAt`
- `cardUpdatedAt`
- `query`
- `position`
- `pageNumber`
- `searchResultsTotal`
- `detailsFetched`
- `searchQuery`
- `contentQuality`
- `detailFetched`
- `scrapedAt`
- `source`
- `contentHash`
- `changeType`

---

## Sample output

One object per product. Here is a real example from a production run:

```json
{
  "listingId": "d63d31b8631f85cf9b160e095b6bf177bd2758c1d7034cb71c555861e108c4eb",
  "type": "product",
  "id": "964495677",
  "nmId": 964495677,
  "imtId": 126892091,
  "title": "Туника домашняя больших размеров короткое",
  "url": "https://www.wildberries.ru/catalog/964495677/detail.aspx",
  "portalUrl": "https://www.wildberries.ru/catalog/964495677/detail.aspx",
  "brand": "Iren Renetti",
  "brandId": 309884,
  "category": "Туники",
  "parentCategory": "Одежда"
}
```

*Truncated — full records contain 75 fields. See Output fields for the complete schema.*

**[Try Wildberries.ru Product Scraper [Just 💰$0.23] now — $5 free credit, no credit card →](https://apify.com/blackfalcondata/wildberries-ru-scraper?fpr=1h3gvi)**

---

## Pricing

Pay only for what you extract. No subscription required — Apify's free $5 credit covers thousands of results.

| Event | Price (USD) |
| --- | --- |
| Actor Start | $0.01 |
| Result | $0.000225 |

See the [actor on Apify](https://apify.com/blackfalcondata/wildberries-ru-scraper?fpr=1h3gvi) for current pricing.

---

## FAQ

**How do I scrape Wildberries.ru?**
Use this actor on Apify to extract structured data from Wildberries.ru. Configure your search query and filters in the input, then click Start — no coding required.

**How do I get Wildberries.ru data as JSON, CSV, or Excel?**
The actor writes each product to Apify's dataset. Download as JSON, CSV, or Excel from the Console, stream via the API, or push to Make, Zapier, Airbyte, or Keboola.

**Is it legal to scrape Wildberries.ru?**
Web scraping of publicly available data is generally legal. This actor only accesses publicly visible information. Always check Wildberries.ru's terms of service for your specific use case.

**How much does it cost?**
Pay-per-event pricing — you only pay for ${NOUNS} extracted. Apify's free $5 credit is enough to run thousands of results before you pay anything.

**How does incremental mode work?**
Each product gets a content hash. On subsequent runs, only new or changed products are emitted — saving time, compute, and storage. Expired products can be tracked separately.

**Do I need an API key or credentials?**
No. Just sign up for Apify, paste your input, and click Start. No credit card required.

---

## Related products by Black Falcon Data

[Browse all Black Falcon Data actors →](https://apify.com/blackfalcondata?fpr=1h3gvi)

---

## Getting started with Apify

New to Apify? [Create a free account with $5 credit](https://console.apify.com/sign-up?fpr=1h3gvi) — no credit card required.

1. Sign up — $5 platform credit included
2. Open [Wildberries.ru Product Scraper [Just 💰$0.23]](https://apify.com/blackfalcondata/wildberries-ru-scraper?fpr=1h3gvi) and configure your input
3. Click **Start** — export results as JSON, CSV, or Excel

Need more later? [See Apify pricing](https://apify.com/pricing?fpr=1h3gvi).

---

## About Black Falcon Data

Black Falcon Data builds production-grade web scrapers for job boards and marketplace data. Browse our full actor catalog at [www.blackfalcondata.com](https://www.blackfalcondata.com).

---

*Last updated: 2026 11*
