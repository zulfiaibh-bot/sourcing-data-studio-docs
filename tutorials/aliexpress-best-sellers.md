---
layout: page
title: "How to find AliExpress best sellers for a keyword, with sold counts and ratings"
description: "List the AliExpress products with the most orders for a keyword, with sold counts, ratings and prices, and learn what the rows can and cannot tell you."
permalink: /tutorials/aliexpress-best-sellers/
---

Which products sell most on AliExpress for a keyword? This guide pulls sold counts, ratings and prices as rows with one call to the Actor [`sourcing-data-studio/aliexpress-search-api`](https://apify.com/sourcing-data-studio/aliexpress-search-api), then shows what the rows can and cannot tell you.

The rows were collected on 9 October 2026 from public search result pages, without logging in, through the default Apify Proxy. Prices are in US dollars unless a line says otherwise. The Python code was run as printed with version 3.3.0 of Apify's Python client.

## What you need

- An Apify account. The Actor is paid per saved result: the two runs below cost $0.18 and $0.90, which fits inside the monthly credit of Apify's free plan (see What it costs).
- For Python: Python 3.11 or later and an API token. In Apify Console the token is under Settings, API & Integrations.

## The input

```json
{
  "searchQueries": ["yoga mat"],
  "maxItems": 60,
  "sort": "orders"
}
```

`searchQueries` holds the keywords. `maxItems` is the maximum number of results to save. `sort` set to `orders` asks AliExpress for the products with the most orders first. AliExpress does the sorting, not the Actor, and in our runs it sorted one result page at a time. More on that below.

## Run it

**Option 1: in the browser.** Open the example task [Find AliExpress best sellers for a keyword](https://apify.com/sourcing-data-studio/aliexpress-search-api/examples/find-aliexpress-best-sellers) and press **Try for free**. The task carries this input. Press **Start**. The rows appear in the Output tab, and you can download them as JSON, CSV or Excel.

**Option 2: Python.** Install the client and set your token in the environment variable `APIFY_TOKEN`:

```bash
pip install "apify-client>=3"
export APIFY_TOKEN="your-token-here"
```

In PowerShell the second line is `$env:APIFY_TOKEN = "your-token-here"`.

Save this script as `best_sellers.py` and run `python best_sellers.py`:

```python
import os

from apify_client import ApifyClient

client = ApifyClient(token=os.environ["APIFY_TOKEN"])

run_input = {
    "searchQueries": ["yoga mat"],
    "maxItems": 60,
    "sort": "orders",
}

run = client.actor("sourcing-data-studio/aliexpress-search-api").call(run_input=run_input)
if run is None:
    raise SystemExit("The run could not be read.")
print(run.status, "|", run.status_message)

rows = list(client.dataset(run.default_dataset_id).iterate_items())

print(len(rows), "rows saved")
for row in rows[:10]:
    print(
        row["position"],
        row["salesCount"],
        row["rating"],
        row["price"],
        row["currency"],
        row["priceType"],
        row["title"][:60],
        sep=" | ",
    )
```

It prints the status and the message of the run, the number of rows, and the first 10 rows in the order AliExpress gave them. While the run is going, the client also shows the Actor's log. On Windows, if you send the output to a file, start it as `python -X utf8 best_sellers.py`: a title can hold characters that the default file encoding cannot write.

## Three real rows

The first three rows of our run of this input, trimmed. Titles are cut to 60 characters. `salesSignal` is the sold text AliExpress shows. `salesCount` is the count as a number: the page data carries the exact figure, and the page shows it rounded. `originalPrice` is the crossed-out price, `null` when none is shown.

```json
[
  {
    "position": 1,
    "title": "Yoga Mat Pilates Fitness Mat 3/4/6mm Thicknes Non Slip Yoga ",
    "price": 0.99,
    "currency": "USD",
    "priceType": "new_user_deal",
    "originalPrice": null,
    "rating": 4.9,
    "salesSignal": "10K+ sold",
    "salesCount": 12665
  },
  {
    "position": 2,
    "title": "Yoga Kneeling Mat Thickened Flat Support Mat Knee Pad Portab",
    "price": 4.33,
    "currency": "USD",
    "priceType": "regular",
    "originalPrice": null,
    "rating": 4.9,
    "salesSignal": "9K+ sold",
    "salesCount": 9099
  },
  {
    "position": 3,
    "title": "4MM Thick EVA Yoga Mats Anti-slip Sport Fitness Mat Blanket ",
    "price": 0.99,
    "currency": "USD",
    "priceType": "new_user_deal",
    "originalPrice": null,
    "rating": 4.5,
    "salesSignal": "7K+ sold",
    "salesCount": 7154
  }
]
```

## What the rows tell you

The top 10 rows by position:

| Position | Title (first 60 characters) | Sold count | Rating | Price (USD) | priceType |
|---|---|---|---|---|---|
| 1 | Yoga Mat Pilates Fitness Mat 3/4/6mm Thicknes Non Slip Yoga  | 12,665 | 4.9 | 0.99 | `new_user_deal` |
| 2 | Yoga Kneeling Mat Thickened Flat Support Mat Knee Pad Portab | 9,099 | 4.9 | 4.33 | `regular` |
| 3 | 4MM Thick EVA Yoga Mats Anti-slip Sport Fitness Mat Blanket  | 7,154 | 4.5 | 0.99 | `new_user_deal` |
| 4 | Foldable Yoga Mat Mute Eco Friendly Folding Travel Fitness E | 4,376 | 4.9 | 14.15 | `regular` |
| 5 | 30x30cm 16 Pcs Interlocking Yoga Mat Home Gym and Yoga Equip | 3,790 | 4.9 | 6.75 | `new_user_deal` |
| 6 | Balance Training Pad Non-Slip High Rebound Thickened Foam Ma | 2,918 | 4.9 | 0.99 | `new_user_deal` |
| 7 | Anti-slip yoga MATS, Pilates fitness MATS, eco-friendly, wom | 2,867 | 4.9 | 6.04 | `new_user_deal` |
| 8 | 30x30cm 16 Pcs Interlocking Yoga Mat Home Gym and Yoga Equip | 2,815 | 4.6 | 6.82 | `new_user_deal` |
| 9 | Balance Pad Non-Slip Foam Mat Ankles Knee Cushion For Core A | 2,784 | 4.9 | 12.57 | `regular` |
| 10 | 4mm Thick TPE Foldable Yoga Mat Easy to Store Travel Exercis | 2,722 | 4.6 | 3.11 | `new_user_deal` |

**One page is sorted. It is not the ranking for the keyword.** In this run the first 59 rows fall from 12,665 sold to 9. The last row is the first result of the second page, and it has 7,653 sold: it would rank third. (The run read a second page because one result of the first page showed no price and was not saved.) In our runs AliExpress sorted each result page of about 60 products and started again on the next page.

**Many prices are new-user deals.** 46 of the 60 rows are `new_user_deal` and 14 are `regular`. AliExpress marks the 46 as deals for new users, and 24 of them show exactly 0.99. Compare prices only between rows of one run with the same `priceType`:

| priceType | Rows | Lowest | Median | Highest |
|---|---|---|---|---|
| `new_user_deal` | 46 | 0.99 | 0.99 | 32.72 |
| `regular` | 14 | 4.32 | 10.955 | 49.47 |

With an even count, the median is the mean of the two middle prices. A third value, `unknown`, marks a result that carries no deal information, so it cannot be told whether its price is a deal. This run had none; the 300-row run below had 4.

## For a ranking, read several pages and sort the rows yourself

In the script, change `"maxItems": 60` to `"maxItems": 300` and replace everything after the `rows = ...` line with this:

```python
rows.sort(key=lambda row: row["salesCount"] or 0, reverse=True)

print(len(rows), "rows saved")
for row in rows[:20]:
    print(row["salesCount"], row["rating"], row["price"], row["currency"], row["title"][:60], sep=" | ")
```

We ran this input for "yoga mat" on the same day. The 300 rows came from 7 result pages. The saved rows of each page were in falling order, and each page began again above where the page before ended: the pages started at 12,665, 9,086, 6,366, 5,974, 3,707, 330 and 496 sold. Of the 60 highest sold counts among the 300 rows, 36 were on the first page. The other 24 were further down. Of the 420 results on those pages, 87 repeated an earlier one and were skipped, not charged, and 33 more came after the limit of 300 was reached. That run came back in euros.

## Keep only products with good ratings

Set `minRating` and `minSoldCount` to keep only rows that reach both:

```json
{
  "searchQueries": ["yoga mat thick"],
  "maxItems": 40,
  "sort": "orders",
  "minRating": 4.5,
  "minSoldCount": 100
}
```

It saved 40 rows. The lowest rating among them is 4.5 and the lowest sold count is 110. The run message says 74 results were left out and not charged. [Example task](https://apify.com/sourcing-data-studio/aliexpress-search-api/examples/aliexpress-top-rated-best-sellers).

## Stay inside a price range

Set `minPrice` and `maxPrice`:

```json
{
  "searchQueries": ["led strip lights"],
  "maxItems": 60,
  "minPrice": 2,
  "maxPrice": 5
}
```

This input has no `sort`, so the results come in the default order of AliExpress. The run saved 60 rows, all priced from 2.03 to 4.86. The range is read in the currency AliExpress shows for the request, which the Actor cannot choose, so check `currency`: this run came back in US dollars. In this run the range was applied to the price shown, so deal prices counted (16 of the 60). The crossed-out price can lie outside it: 52 rows had an `originalPrice` above 5. [Example task](https://apify.com/sourcing-data-studio/aliexpress-search-api/examples/aliexpress-products-in-a-price-range).

## What it costs

You pay per saved result: $3.00 per 1,000 on the Free tier, so 60 rows cost $0.18 and 300 rows cost $0.90. Results left out by a minimum cost nothing. Apify adds a tiny start fee per run, and reading the stored rows is billed as normal platform usage.

## What to watch for

Points 1 to 3 and point 7 are what AliExpress showed a first-time visitor who was not logged in, in our runs of that day. Points 4 to 6 are about what the Actor saves.

1. **The price is a first-visit price, and it can move.** The Actor reads AliExpress like a first-time visitor. In the US-dollar "yoga mat" run 46 of 60 prices were new-user deals; in the two runs here that came back in euros (300 and 120 rows) none was. `priceType` flags them. `originalPrice` holds the crossed-out price when one is shown: in that "yoga mat" run it was empty on 59 of the 60 rows, while a run of the same search four minutes earlier had it on 58 of 60. Between those two runs the 60 products were the same, and 48 of them showed a different price. Compare prices inside one run, not between runs.
2. **Language, currency and item ids changed from run to run.** One of our runs came back with English titles and US dollars; a run of the same search fourteen minutes later came back with German titles, euros and different item ids. AliExpress chooses them for each request, apparently by the address the request comes from, and the Actor does not choose that address. Read `currency`, never add prices in different currencies, and do not match two runs by item id.
3. **A sort order held inside one page only.** See above. For a ranking, read several pages and sort.
4. **Some top sellers can be missing.** Results shown as a free gift with no price are not saved or charged. In the "yoga mat" run one such result was left out, at position 34. In a run for "usb c cable" it was 43 of 180.
5. **A minimum drops rows with gaps.** A result with no rating is left out when you set a minimum rating, and one with no sold count when you set a minimum sold count; neither is charged. With no minimum set, the price-range run had 15 of 60 rows with no rating and 4 with no sold count.
6. **Search pages only.** The rows carry no ship-to country, variants, reviews or seller data.
7. **The same title can fill several rows.** The same title can come under several item ids, each with its own sold count and price. In the 60-row run the last row, which would rank third, carries the same title as the row at position 2, and rows 5 and 8 of the table above share one title. In the 300-row run 17 titles stood on more than one row, and 6 of the 20 highest sold counts repeated a title already higher in the list.

Corrected on 9 October 2026, the day it was published: the Python example failed with version 3 of Apify's Python client and now runs with it; some statements about AliExpress were narrowed to what our runs showed; point 7 is new.

Unofficial: not affiliated with or endorsed by AliExpress or Alibaba Group.
