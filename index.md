---
layout: home
title: Sourcing Data Studio
---

Data APIs for importers and sellers who buy from China and sell in the Gulf and beyond. Each API runs on the [Apify Store](https://apify.com/sourcing-data-studio), returns clean rows, and charges only for results that are delivered.

## APIs

| API | What it returns | Source | Where |
|---|---|---|---|
| AliExpress Search API | Search results for a keyword: price as shown, new-user deal flag, rating, sold count, image and link | Public search pages of aliexpress.com | [On the Apify Store](https://apify.com/sourcing-data-studio/aliexpress-search-api) |
| Shopify Products API | The public product catalogue of a store: prices, compare-at prices, variants, availability and images | The catalogue feed that Shopify stores list for read-only software agents | [On the Apify Store](https://apify.com/sourcing-data-studio/shopify-products-api) |
| EU Import Data API | Monthly EU trade by product code and partner country, with unit values | Eurostat Comext | [On the Apify Store](https://apify.com/sourcing-data-studio/eu-import-data-api) |
| Bahrain Import Data API | Monthly imports by HS code and origin country, with unit values | Bahrain Open Data Portal | [On the Apify Store](https://apify.com/sourcing-data-studio/bahrain-import-data-api) |
| HS Code Lookup API | Tariff code, description and duty rate for a product | UK Trade Tariff, US Harmonized Tariff Schedule | [On the Apify Store](https://apify.com/sourcing-data-studio/hs-code-lookup-api) |
| EU Safety Gate API | EU alerts on dangerous non-food products by keyword and origin | European Commission Safety Gate | [On the Apify Store](https://apify.com/sourcing-data-studio/eu-safety-gate-api) |
| CPSC Recall API | US consumer product recalls by product, date and maker country | US Consumer Product Safety Commission | [On the Apify Store](https://apify.com/sourcing-data-studio/cpsc-recall-api) |
| Sanctions Screening API | Company, vessel and aircraft names checked against US and UK restricted-party lists | US Consolidated Screening List, UK Sanctions List | [On the Apify Store](https://apify.com/sourcing-data-studio/sanctions-screening-api) |
| Company LEI Lookup API | Legal entity records by company name or LEI code | Global LEI Index (GLEIF) | [On the Apify Store](https://apify.com/sourcing-data-studio/lei-lookup-api) |
| Landed cost and margin layer | Duty, VAT, freight band, marketplace fees and margin for Bahrain, UAE and Saudi Arabia | Own tables | On hold: no openly licensed per-HS duty source for Bahrain, the UAE or Saudi Arabia was found (checked 2026-10-06) |

Every API is unofficial: none is affiliated with or endorsed by the organisation whose data it reads.

## How the APIs work

- Pay per result: failed requests, duplicates and incomplete rows are never charged.
- Use them from Apify Console, the Apify API, n8n, Make or AI agents through the Apify MCP server.
- Public data only: no logins, no accounts, no personal data. Contact persons, phone numbers and email addresses are removed.
- Results from open-data sources carry the source name and the retrieval date, and the licence line the source asks for.

## Tutorials

- [How to find AliExpress best sellers for a keyword, with sold counts and ratings](tutorials/aliexpress-best-sellers/)

## For website and data owners

Our software says what it is. Requests to data services carry a user agent that starts with `SourcingDataStudio/`, followed by the name of the API and the address of this page. Where a page needs a browser, we use an unmodified headless Chromium browser, which announces itself as `HeadlessChrome`.

We read public pages and official open-data endpoints at a low rate. We do not log in, do not use other people's accounts or keys, and do not solve or bypass CAPTCHA or verification pages. When a site refuses a request, we stop: a refused or blocked page is not asked for again in the same run, and never from another address.

To report a problem, or to ask us to stop reading your site, open an issue at [github.com/zulfiaibh-bot/sourcing-data-studio-docs/issues](https://github.com/zulfiaibh-bot/sourcing-data-studio-docs/issues). Requests to stop are honoured.
