---
layout: home
title: Sourcing Data Studio
---

Data APIs for importers and sellers who buy from China and sell in the Gulf and beyond. Each API runs on the [Apify Store](https://apify.com/store), returns clean rows, and charges only for results that are delivered.

## APIs

| API | What it returns | Source | Status |
|---|---|---|---|
| Bahrain Import Data API | Monthly imports by HS code and origin country, with unit values | Bahrain Open Data Portal | in testing |
| EU Import Data API | Monthly EU trade by product code and partner country, with unit values | Eurostat Comext | in testing |
| HS Code Lookup API | Tariff code, description and duty rate for a product | UK Trade Tariff, US Harmonized Tariff Schedule | in testing |
| Sanctions Screening API | Company names checked against US and UK restricted-party lists | US Consolidated Screening List, UK Sanctions List | in testing |
| Company LEI Lookup API | Legal entity records by company name or LEI code | Global LEI Index (GLEIF) | in testing |
| CPSC Recall API | US consumer product recalls by product, date and maker country | US Consumer Product Safety Commission | in testing |
| EU Safety Gate API | EU alerts on dangerous non-food products by keyword and origin | European Commission Safety Gate | in testing |
| Landed cost and margin layer | Duty, VAT, freight band, marketplace fees and margin for Bahrain, UAE and Saudi Arabia | Own tables | planned |

Every API is unofficial: none is affiliated with or endorsed by the organisation whose data it reads. A link to each API's Store page appears here when it is published.

## How the APIs work

- Pay per result: failed requests, duplicates and incomplete rows are never charged.
- Use them from Apify Console, the Apify API, n8n, Make or AI agents through the Apify MCP server.
- Public data only: no logins, no accounts, no personal data. Contact persons, phone numbers and email addresses are removed.
- Results from open-data sources carry the source name and the retrieval date, and the licence line the source asks for.

## For website and data owners

Our software says what it is. Requests to data services carry a user agent that starts with `SourcingDataStudio/`, followed by the name of the API and the address of this page. Where a page needs a browser, we use an unmodified headless Chromium browser, which announces itself as `HeadlessChrome`.

We read public pages and official open-data endpoints at a low rate. We do not log in, do not use other people's accounts or keys, and do not solve or bypass CAPTCHA or verification pages. When a site refuses a request, we stop.

To report a problem, or to ask us to stop reading your site, open an issue at [github.com/zulfiaibh-bot/sourcing-data-studio-docs/issues](https://github.com/zulfiaibh-bot/sourcing-data-studio-docs/issues). Requests to stop are honoured.
