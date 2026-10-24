# twitter scraper python: Four Routes Compared, Where ScraperAPI Fits, and What a Plan Actually Costs

Most people searching "twitter scraper python" want the same thing: pull tweets, profiles, or search results from X into a Python script without the thing breaking every two weeks. The short answer for 2026 is that you have four realistic routes — the official X API, the guest-token method, open-source libraries like twscrape, and a managed scraping API such as ScraperAPI. Which one fits depends on your volume, your tolerance for maintenance, and whether you need parsed JSON or are willing to parse HTML yourself.

This guide walks through all four, then goes deep on the managed-API option with ScraperAPI's full plan table, its credit-cost mechanics (the part most reviews skip), and who shouldn't buy it.

> **Quick take:** if you're already running Python scripts that hit arbitrary websites and just want X pages to stop throwing CAPTCHAs at you, ScraperAPI's Hobby plan at $49/month (100,000 credits, 20 concurrent requests) is the entry point. You can test it first with 1,000 free credits plus a 7-day, 5,000-request trial — no credit card needed. 👉 [Start the 7-day free trial and test it on your own X targets](https://www.scraperapi.com/?fp_ref=coupons)

## The four ways to scrape Twitter/X with Python right now

X killed its free API tier in 2023, and that's why this keyword even exists. Here's what each route looks like today, based on a June 2026 hands-on comparison of live X targets:

**1. The official X API (via Tweepy or raw HTTP).** The v2 API moved to pay-per-usage pricing in 2026: credits are deducted per request, and the same resource requested twice within 24 hours is only charged once. The old fixed tiers — Basic at $200/month, Pro at $5,000/month — are retired for new sign-ups and only survive for existing subscribers. Enterprise access starts around $42,000/month. The new free tier gives you 500 posts per month with no meaningful search. For a few thousand posts, pay-per-use is workable; for volume collection, the bill climbs fast.

**2. The guest-token method.** You POST to X's guest-activation endpoint with the public bearer token embedded in X's own frontend JavaScript, get a token back in one request, and start hitting internal endpoints without logging in. It genuinely works — one June 2026 test pulled a public profile and its tweets this way on the first attempt. The catch: tokens expire every 2–4 hours, the internal `doc_id` parameters rotate every few weeks, and datacenter IPs slam into a 300-requests-per-hour limit quickly. One widely-used guest-token library doesn't even import on Python 3.13 anymore — it crashes with an `AttributeError` tied to a Python internal that was removed years ago.

**3. Open-source libraries.** twscrape is the most maintained option: an async Python library and CLI with 2,877 GitHub stars, multi-account rotation, and built-in rate-limit handling. The trade-off is that it runs on *your own account pool* — you supply the X accounts, manage the sessions in SQLite, and absorb the ban risk yourself.

**4. A managed scraping API.** You send a request with your API key, the service handles proxies, CAPTCHAs, and rendering, and you get the page back. This is the category ScraperAPI sits in.

There's also a fifth oddball worth knowing: X's syndication endpoint (`syndication.twitter.com`) powers embedded timelines and requires no token at all — a test in June 2026 got a `200` with roughly 121,000 bytes containing per-tweet `full_text`, `favorite_count`, and `screen_name` fields. It only covers public timeline embeds, but for quick profile pulls it costs nothing.

## Where ScraperAPI fits in that picture

ScraperAPI is a general-purpose scraping API, and that shapes exactly how you'd use it for Twitter work. You point it at any X URL, it routes the request through its proxy pools, handles CAPTCHA and anti-bot detection, optionally renders JavaScript, and returns the page. All plans include JS rendering, premium proxies, automatic retries, custom sessions, and a 99.9% uptime guarantee.

Two limitations matter before you commit, and almost no ranking article spells them out:

- **You get raw HTML, not structured Twitter JSON.** ScraperAPI has no dedicated Twitter endpoints. Your Python code does the parsing — BeautifulSoup against the page's meta tags and embedded JSON, for example. That's fine for profile pages and individual tweet pages; it's more work for anything search-shaped.
- **X is an aggressive anti-bot target.** The service's own pricing documentation notes that sites behind Cloudflare, Datadome, or PerimeterX add 10 credits per request when the bypass fires. X pages change their defenses frequently, which is why a third-party comparison of five Twitter scraper tools concluded that same-volume Twitter work tends to cost more on a general proxy-based API than on a dedicated Twitter API, and called HTML-based X scraping "especially fragile." That assessment is fair — ScraperAPI is the right tool when you already have Python scraping code and want it to keep working across many sites, X included, rather than when Twitter is your only and highest-volume target.

If you're unsure how it behaves on your specific URLs, the dashboard has an API Playground where you can test a target URL and see the actual credit usage before running anything at scale. 👉 [Sign up and try the API Playground on your X URLs before paying anything](https://www.scraperapi.com/?fp_ref=coupons)

## Full plan and pricing table

Every paid plan includes the same core feature set (JS rendering, premium proxies, auto-parsing, retries, unlimited bandwidth). Annual billing takes 10% off. Prices below are USD, per month.

| Plan | Credits/mo | Concurrency | Monthly price | Annual (per mo) | Buy |
| --- | --- | --- | --- | --- | --- |
| Hobby | 100,000 | 20 | $49 | $44.10 | [ 开通 Hobby](https://www.scraperapi.com/pricing/?fp_ref=coupons) |
| Startup | 1,000,000 | 50 | $149 | $134.10 | [ 开通 Startup](https://www.scraperapi.com/pricing/?fp_ref=coupons) |
| Business | 3,000,000 | 100 | $299 | $269.10 | [ 开通 Business](https://www.scraperapi.com/pricing/?fp_ref=coupons) |
| Scaling (most popular) | 5,000,000 | 200 | $475 | $427.50 | [ 开通 Scaling](https://www.scraperapi.com/pricing/?fp_ref=coupons) |
| Professional | 10,500,000 | 300 | $975 | $877.50 | [ 开通 Professional](https://www.scraperapi.com/pricing/?fp_ref=coupons) |
| Advanced | 21,500,000 | 500 | $1,975 | $1,777.50 | [ 开通 Advanced](https://www.scraperapi.com/pricing/?fp_ref=coupons) |
| Enterprise | 22M+ (custom) | 500+ | Custom quote | Custom quote | [ 联系销售获取报价](https://www.scraperapi.com/contact-sales/?fp_ref=coupons) |

Beyond the paid tiers there's a permanent free plan: 1,000 credits per month with up to 5 concurrent connections, plus 5,000 free requests during your first 7 days. Documentation also lists a Student plan tier; its pricing wasn't published on the pages reviewed here, so check the pricing page directly if that applies to you. If you need more test credits, support can grant additional ones on request.

Geotargeting is a real difference between tiers, not a marketing line: Hobby and Startup are limited to US and EU locations, while Business and above unlock country-level geotargeting. Hobby's analytics dashboard also only retains 30 days of data.

## What a credit actually costs on X — the math most reviews skip

"One credit = one standard page request" is the headline, but the real consumption depends on the target domain and your parameters. Confirmed multipliers from ScraperAPI's cost documentation: normal requests 1 credit, Amazon 5, Google/Bing 25, LinkedIn 30. Social media domains are priced in this higher band — LinkedIn sits at 30 — and while the exact multiplier for x.com isn't published in the public cost table, you don't have to guess. Two ways to check before you spend anything:

1. Open the **Domain Multiplier** in your dashboard and look up any X URL's credit cost.
2. Call the cost endpoint directly: `https://api.scraperapi.com/account/urlcost?api_key=YOUR_KEY&url=<target-url>` — it accepts all parameters, including `render=true`, and returns the exact cost.

Parameter add-ons stack on top of the domain multiplier, and they change the math dramatically:

| Parameter | Extra cost per request |
| --- | --- |
| `render=true` | +10 credits |
| `premium=true` | +10 credits |
| `screenshot=true` | +10 credits |
| `ultra_premium=true` | +30 credits |
| `premium=true + render=true` | +25 credits |
| Cloudflare / Datadome / PerimeterX / Akamai bypass | +10 credits when triggered |

A rendered, premium request through a bypassed protection layer isn't 1 credit — it's 36 or more. On the Hobby plan's 100,000 credits, that's the difference between ~100,000 plain requests and fewer than 3,000 heavy ones. This is the single biggest cost variable in any twitter scraper python project, and it's why you should set `max_cost` per request: ScraperAPI lets you cap the credit spend of any single scrape so one heavy URL can't drain your balance.

Two billing details worth knowing:

- **Only successful requests are charged** — status codes 200 and 404. Failed anti-bot attempts don't eat your credits. (Requests you cancel within 70 seconds *are* charged, so don't abort impatiently.)
- Every response includes an `sa-credit-cost` header telling you exactly what that request consumed, so you can audit usage from your Python code instead of eyeballing a dashboard.

## What happens when you run out of credits

Credits run out, and what ScraperAPI does next depends on your plan — this is buried in the billing FAQ, and it affects which plan you should pick:

- By default, scraping **pauses** until your credits renew.
- On Hobby, Startup, Business, Scaling, and Professional (monthly), you can enable **Auto-Upgrade**: when you hit 100% usage, the account automatically moves to the next tier, which usually has a better price-per-credit. It's opt-in.
- On Scaling, Professional, Advanced, and Enterprise, you get **Pay-As-You-Go**: keep scraping past your limit at a fixed per-credit rate, up to a monthly spending cap you set yourself. The cap is a spending limit, not a prepayment.
- Professional and Advanced monthly plans include Pay-As-You-Go by default.
- Annual plans can renew early for a fresh batch of credits.

Exceeding your concurrency limit returns a 429 and the request is rejected outright — no queuing, no charge. If your script fires 30 parallel requests on Hobby's 20-concurrent limit, a third of them just fail. Match concurrency to the plan or queue on your side.

## Refunds, cancellation, and the risk-free part

ScraperAPI has a 7-day no-questions-asked refund policy — contact support and they process it. You can also cancel anytime from the dashboard at no charge. Combined with the no-credit-card trial, you can verify the whole pipeline on your actual X targets before a single dollar is at risk. That matters more here than with most tools, because whether ScraperAPI works well on X for *your* specific use case (which pages, which volume, rendered or not) is something only a live test will tell you. 👉 [查看全部套餐价格并开通](https://www.scraperapi.com/pricing/?fp_ref=coupons)

## How the alternatives compare on price

For context, here's what the competing routes cost, using only verified figures:

| Route | Entry cost | What you get |
| --- | --- | --- |
| X official API (free tier) | $0 | 500 posts/month read, no meaningful search |
| X API via Tweepy (legacy tiers) | $100/mo Basic (10K posts), $5K/mo Pro (1M) | Clean JSON; follower graph needs the $5K tier; rate limits as low as 1 req/15s; Python only |
| twscrape (open source) | $0 | Async Python, your own account pool, you absorb ban risk |
| Dedicated Twitter APIs (e.g. FetchLayer, ChocoData, ScrapeCreators) | Free tiers available, pay-per-use | Structured JSON Twitter endpoints; FetchLayer covers followers/following endpoints that cost $5K/mo on the official API |
| Apify | Limited free tier, per actor-run | Marketplace actors for Twitter; quality depends on the individual actor author |
| ScraperAPI | $0 to test, then $49/mo | Works on any URL; returns HTML you parse yourself |

The honest framing: if you need structured Twitter JSON — search results, follower lists, replies as clean fields — a dedicated Twitter API will do that with less code, and the official API's follower-graph access sits behind a $5,000/month tier that makes third parties genuinely attractive for that specific need. ScraperAPI earns its place when Twitter is one of several sites you're scraping, when you want one API key and one billing line for everything, or when your existing Python scraper needs proxies and CAPTCHA handling rather than a rewrite.

## Who should skip ScraperAPI for this

Being straight about fit saves everyone time. Skip ScraperAPI for your Twitter project if:

- You only ever need tweet and profile **data as JSON fields** and will never scrape another site — a dedicated Twitter API does this more directly.
- You need **follower/following graph data at scale** — neither ScraperAPI's HTML approach nor any sub-$5K official tier handles this gracefully.
- Your volume is tiny and episodic — the syndication endpoint or an occasional guest-token script costs nothing, and twscrape with a couple of spare accounts covers hobby projects.

## FAQ

**Does ScraperAPI have a dedicated Twitter/X endpoint?**
No. It's a general scraping API that returns page HTML. There are no Twitter-specific JSON endpoints; parsing happens in your Python code.

**How many credits does scraping x.com cost per request?**
The public cost table lists normal (1), Amazon (5), SERP (25), and LinkedIn (30) multipliers but doesn't publish x.com's exact rate. Check it precisely with the Domain Multiplier dashboard tool or the `account/urlcost` API endpoint before committing — both accept parameters like `render=true` so you see the true cost.

**Can I try it without paying?**
Yes. The free plan gives 1,000 credits per month with 5 concurrent connections, and your first 7 days add 5,000 free requests. No credit card required at signup.

**Is annual billing worth it?**
Annual takes 10% off: Hobby drops from $49 to $44.10 per month, Scaling from $475 to $427.50. Over a year, Hobby saves about $58 and Scaling about $570. Only take annual after the trial confirms the service works on your targets.

**What if it doesn't work for my use case?**
Cancel from the dashboard anytime at no charge, or use the 7-day no-questions-asked refund if you've already paid.

## Picking your plan, in one pass

Match the decision to your situation:

- **Testing or evaluating** → free plan + 7-day trial. Nothing to decide yet.
- **Small, steady Twitter monitoring in Python** (a few thousand page pulls a month) → Hobby at $49/month, 100K credits, with `max_cost` set per request.
- **Twitter plus other sites in one pipeline** → Startup or Business, depending on whether you need 1M or 3M credits and country-level geotargeting.
- **Heavy volume with spiky months** → Scaling, which is both the officially most popular tier and the cheapest one with Pay-As-You-Go headroom at $475/month.
- **Mostly JSON data, minimal scraping of anything else** → ScraperAPI isn't your best first stop; look at dedicated Twitter APIs or the official pay-per-use model instead.

One last check before you buy: run a handful of your real target URLs through the API Playground and note the `sa-credit-cost` header on each. Multiply by your monthly volume, compare against the plan table, and the right tier picks itself. 👉 [开通前先用免费额度实测你的 X 目标 URL](https://www.scraperapi.com/?fp_ref=coupons)
