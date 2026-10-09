---
layout: page
title: "How to find AliExpress best sellers for a keyword, with sold counts and ratings"
description: "List the AliExpress products with the most orders for a keyword, with sold counts, ratings and prices, and learn how to read the rows without being misled."
permalink: /tutorials/aliexpress-best-sellers/
---

Which products sell most on AliExpress for a keyword? This guide pulls sold counts, ratings and prices as rows with one call to the Actor [`sourcing-data-studio/aliexpress-search-api`](https://apify.com/sourcing-data-studio/aliexpress-search-api), then shows how to read them without being misled. That part helps even if you never run anything.

The rows were collected on 9 October 2026 through the default Apify Proxy. Prices are in US dollars unless a line says otherwise.

## What you need

- An Apify account. The free plan is enough.
- For Python: an API token and Python 3.

## The input

```json
{
  "searchQueries": ["yoga mat"],
  "maxItems": 60,
  "sort": "orders"
}
```

`searchQueries` holds the keywords. `maxItems` is the most results to save. `sort` set to `orders` asks AliExpress for the products with the most orders first. AliExpress does the sorting, not the Actor, and it sorts one result page at a time. More on that below.

## Run it

**Option 1: in the browser.** Open the example task [Find AliExpress best sellers for a keyword](https://apify.com/sourcing-data-studio/aliexpress-search-api/examples/find-aliexpress-best-sellers) and press **Try for free**. It starts with this input. Press **Start**. The rows appear in the Output tab, and you can download them as JSON, CSV or Excel.

**Option 2: Python.** Install the client, set your token in the environment variable `APIFY_TOKEN`, and run the script.

```bash
pip install apify-client
export APIFY_TOKEN="your-token-here"   # PowerShell: $env:APIFY_TOKEN = "your-token-here"
```

```python
import os

from apify_client import ApifyClient

client = ApifyClient(os.environ["APIFY_TOKEN"])

run_input = {
    "searchQueries": ["yoga mat"],
    "maxItems": 60,
    "sort": "orders",
}

run = client.actor("sourcing-data-studio/aliexpress-search-api").call(run_input=run_input)
rows = list(client.dataset(run["defaultDatasetId"]).iterate_items())

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

It prints the first 10 rows in the order AliExpress gave them.

## Three real rows

The first three rows of that run, trimmed. Titles are cut to 60 characters. `salesSignal` is the sold text AliExpress shows. `salesCount` is the number the Actor reads. `originalPrice` is the crossed-out price, `null` when none is shown.

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
| 1 | Yoga Mat Pilates Fitness Mat 3/4/6mm Thicknes Non Slip Yoga  | 12,665 | 4.9 | 0.99 | new_user_deal |
| 2 | Yoga Kneeling Mat Thickened Flat Support Mat Knee Pad Portab | 9,099 | 4.9 | 4.33 | regular |
| 3 | 4MM Thick EVA Yoga Mats Anti-slip Sport Fitness Mat Blanket  | 7,154 | 4.5 | 0.99 | new_user_deal |
| 4 | Foldable Yoga Mat Mute Eco Friendly Folding Travel Fitness E | 4,376 | 4.9 | 14.15 | regular |
| 5 | 30x30cm 16 Pcs Interlocking Yoga Mat Home Gym and Yoga Equip | 3,790 | 4.9 | 6.75 | new_user_deal |
| 6 | Balance Training Pad Non-Slip High Rebound Thickened Foam Ma | 2,918 | 4.9 | 0.99 | new_user_deal |
| 7 | Anti-slip yoga MATS, Pilates fitness MATS, eco-friendly, wom | 2,867 | 4.9 | 6.04 | new_user_deal |
| 8 | 30x30cm 16 Pcs Interlocking Yoga Mat Home Gym and Yoga Equip | 2,815 | 4.6 | 6.82 | new_user_deal |
| 9 | Balance Pad Non-Slip Foam Mat Ankles Knee Cushion For Core A | 2,784 | 4.9 | 12.57 | regular |
| 10 | 4mm Thick TPE Foldable Yoga Mat Easy to Store Travel Exercis | 2,722 | 4.6 | 3.11 | new_user_deal |

**One page is sorted. It is not the top list.** In this run the first 59 rows fall from 12,665 sold to 9. The last row is the first result of the second page, and it has 7,653 sold: it would rank third. AliExpress sorts each result page of about 60 products and starts again on the next page.

**Many prices are one-time deals.** 46 of the 60 rows are `new_user_deal` and 14 are `regular`. 24 of the 46 deals show exactly 0.99. A deal price applies once, to a first order. Compare prices only between rows of one run with the same `priceType`:

| priceType | Rows | Lowest | Median | Highest |
|---|---|---|---|---|
| new_user_deal | 46 | 0.99 | 0.99 | 32.72 |
| regular | 14 | 4.32 | 10.955 | 49.47 |

With an even count, the median is the mean of the two middle prices.

## For a ranking, read several pages and sort the rows yourself

Raise `maxItems`, then sort by `salesCount`:

```python
run_input["maxItems"] = 300
run = client.actor("sourcing-data-studio/aliexpress-search-api").call(run_input=run_input)
rows = list(client.dataset(run["defaultDatasetId"]).iterate_items())

rows.sort(key=lambda row: row["salesCount"] or 0, reverse=True)
for row in rows[:20]:
    print(row["salesCount"], row["rating"], row["price"], row["currency"], row["title"][:60], sep=" | ")
```

We ran this for "yoga mat" on the same day. The 300 rows came from 7 result pages. Each page was in falling order, and each began again above where the page before ended: the pages started at 12,665, 9,086, 6,366, 5,974, 3,707, 330 and 496 sold. Of the 60 highest sold counts among the 300 rows, 36 were on the first page. The other 24 were further down. Of 420 results on those pages, 87 repeated an earlier one and were skipped, not charged. That run came back in euros.

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

Add `minPrice` and `maxPrice`. With 2 and 5 for "led strip lights", the run saved 60 rows, all priced from 2.03 to 4.86. AliExpress applies the range to the price it shows, so deal prices count (16 of the 60). The crossed-out price can lie outside it: 52 rows had an `originalPrice` above 5. [Example task](https://apify.com/sourcing-data-studio/aliexpress-search-api/examples/aliexpress-products-in-a-price-range).

## What it costs

You pay for saved results only: $3.00 per 1,000 on the Free tier, so 60 rows cost $0.18 and 300 rows cost $0.90. Results left out by a minimum cost nothing. Apify also charges a tiny start fee per run.

## What to watch for

Most of this holds when you browse AliExpress by hand as well.

1. **The price is a first-visit price, and it moves.** The Actor reads AliExpress like a first-time visitor, so many prices are one-time new-user deals. `priceType` flags them. `originalPrice` holds the list price when one is shown; in the "yoga mat" run it was empty on 59 of the 60 rows. We ran the same search twice, four minutes apart: the 60 products were the same, and 48 of them showed a different price. Compare prices inside one run, not between runs.
2. **Language, currency and item ids follow the request.** AliExpress picks them from the address the request comes from, and the Actor does not choose that address. One of our runs came back with English titles and US dollars; a run of the same search fifteen minutes later came back with German titles, euros and different item ids. Read `currency`, never add prices in different currencies, and do not match two runs by item id.
3. **A sort order holds inside one page.** See above. For a ranking, read several pages and sort.
4. **Some top sellers can be missing.** Results shown as a free gift with no price are not saved or charged. In the "yoga mat" run one such result was left out, at position 34. In a run for "usb c cable" it was 43 of 180.
5. **Minimums drop rows with gaps.** A result with no rating or no sold count is left out when you set a minimum, and not charged. With no minimum set, the price-range run still had 15 of 60 rows with no rating and 4 with no sold count.
6. **Search pages only.** The rows carry no ship-to country, variants, reviews or seller data.

Unofficial: not affiliated with or endorsed by AliExpress or Alibaba Group.
