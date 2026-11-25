# ScraperAPI vs Scrape.do: Which Is Actually Cheaper, What Do the Credits Really Cost, and Where Does the $29 Plan Stop? (Plans, Geo Limits, Migration Notes)

Scrape.do's Hobby plan hands you 250,000 credits for $29 a month. ScraperAPI's Hobby plan gives you 100,000 for $49, plus 20 concurrent threads instead of 10. Per credit that puts Scrape.do at roughly $0.12 per 1,000 and ScraperAPI at $0.49, a gap of about four times.

That gap is real, and it is also the least interesting number in this comparison. What decides most purchases is how many credits one protected page burns, and which tier you have to buy before residential proxies or global geotargeting switch on. Those two things move the effective price by 10x to 75x, far more than the entry subscription does.

Short version for the impatient: plain HTML at low volume goes to Scrape.do Hobby. Structured Amazon, Google and Walmart endpoints, a 50,000-URL async queue and a hard per-request cost cap go to ScraperAPI Hobby at $49.

> **Quick pick:** Scrape.do Hobby, $29/month for 250,000 credits and 10 concurrent requests, is the cheaper start for ordinary pages and light testing. ScraperAPI Hobby, $49/month for 100,000 credits and 20 concurrent requests, earns the extra $20 when you want parsed endpoints, DataPipeline scheduling and `max_cost` protection against a runaway bill. 👉 [See ScraperAPI's current plans and credit allowances](https://www.scraperapi.com/pricing/?fp_ref=coupons)

## ScraperAPI plans

ScraperAPI runs six tiers. Hobby is the only paid tier whose credit count and concurrency are spelled out in the public documentation, so treat the higher tiers as "check the pricing page for today's numbers" rather than fixed figures. Credit allotments get adjusted with promotions, and a copied table goes stale faster than the page does.

| Plan | Price | Credits / concurrency | What changes at this tier | Sign up |
| --- | --- | --- | --- | --- |
| Free trial | $0 | 5,000 credits over 7 days, 5 concurrent. The pricing FAQ and billing docs also describe an ongoing 1,000-credit free account | Testing; US/EU targeting | [Start the free trial](https://www.scraperapi.com/pricing/?fp_ref=coupons) |
| Hobby | $49/mo | 100,000 credits, 20 concurrent | US/EU geotargeting, 30-day dashboard history | [Open a Hobby account](https://www.scraperapi.com/pricing/?fp_ref=coupons) |
| Startup | Listed on the pricing page | Larger credit pool and concurrency than Hobby | Still US/EU geotargeting | [Compare Startup with Hobby](https://www.scraperapi.com/pricing/?fp_ref=coupons) |
| Business | Listed on the pricing page | Larger again | Global `country_code`, unlimited dashboard history | [See Business tier pricing](https://www.scraperapi.com/pricing/?fp_ref=coupons) |
| Scaling | Volume pricing | Pay-as-you-go overflow past your plan credits | Spiky workloads that outgrow a fixed pool | [Check the Scaling tier](https://www.scraperapi.com/pricing/?fp_ref=coupons) |
| Enterprise | Custom quote | Negotiated | Custom terms. Docs note a floor of $3 per 1,000 unrendered ultra-premium requests and $7 rendered | [Request Enterprise pricing](https://www.scraperapi.com/pricing/?fp_ref=coupons) |

Two details in that table cost people money. First, the free allowance is two separate things: a 7-day 5,000-credit trial and, according to the pricing FAQ and billing documentation, a standing 1,000-credit account with 5 concurrent connections. Confirm the balance you actually see at signup, because the marketing page and the FAQ do not describe the same number.

Second, global geotargeting does not exist below Business. Hobby and Startup are US and EU only. If your project needs `country_code=jp` or `br`, the $49 plan cannot do it at any volume.

## Scrape.do plans

Scrape.do's public lineup, for reference only:

| Plan | Monthly | Credits | Main + async slots | Per 1,000 credits |
| --- | --- | --- | --- | --- |
| Free | $0 | 1,000, refreshed monthly | 5 + 2 | n/a |
| Hobby | $29 | 250,000 | 10 + 3 | $0.11 |
| Pro | $99 | 1,250,000 | 50 + 15 | $0.08 |
| Business | $249 | 3,500,000 | 100 + 30 | $0.07 |
| Advanced | $699 | 10,000,000 | 200 + 60 | $0.06 |
| Enterprise | Custom | Negotiated | Negotiated | Custom |

Annual billing works out to roughly ten months for twelve: $290 for Hobby, $990 for Pro, $2,490 for Business, $6,990 for Advanced. The free tier refreshes 1,000 credits every month with no card, plus 5 main and 2 async slots, which makes it more useful for long-term light testing than a one-week trial.

## What a request actually costs

The headline rate applies to one kind of request: a static HTML page, no JavaScript, no premium proxy, no anti-bot challenge. Everything else multiplies.

ScraperAPI's own documentation lists the multipliers. `render=true` adds 10 credits, `premium=true` adds 10, `ultra_premium=true` adds 30, a screenshot adds 10, and each anti-bot bypass (Cloudflare, Cloudflare Turnstile, DataDome, PerimeterX, Akamai) adds 10 when it triggers automatically. Domain rules stack on top: Amazon, Walmart and eBay cost 5 credits, Google and Bing cost 25, LinkedIn costs 30.

The part that catches people is that combinations do not add up linearly. `premium=true` plus `render=true` costs 25 credits, not 20. `ultra_premium=true` plus `render=true` costs 75, not 40. Nearly double what the individual line items suggest.

Run those numbers against a $49 Hobby plan and the sticker price stops meaning much:

| Request type | ScraperAPI credits | Scrape.do credits | ScraperAPI Hobby, $ per 1K usable | Scrape.do Hobby, $ per 1K usable |
| --- | --- | --- | --- | --- |
| Standard fetch | 1 | 1 | $0.49 | $0.12 |
| JavaScript rendering | 10 | 5 | $4.90 | $0.58 |
| Residential routing | 10 | 10 | $4.90 | $1.16 |
| Residential + rendering | 25 | 25 | $12.25 | $2.90 |
| Ultra premium + rendering | 75 | No published equivalent | $36.75 | Not comparable |

These assume full utilization of the monthly credit pool, which almost nobody achieves. They still show the shape of the problem: the same "residential plus rendering" request costs $12.25 per 1,000 usable responses on ScraperAPI Hobby and $2.90 on Scrape.do Hobby. That is the four-times gap again, but now on a request type that real projects actually run.

Scrape.do's equivalent multipliers are flatter. Amazon URLs cost 1 credit, Google 10, LinkedIn 30, and rendering is 5 credits per datacenter response. A 25-credit mode burns a 1,000-credit free allowance in 40 billable responses, which is worth knowing before you point a test script at Google.

One more billing trap: ScraperAPI runs two price lists. Core API and DataPipeline charge differently for the same parameters, so `premium` plus `render` costs 20 credits on DataPipeline and 25 on Core API. DataPipeline's base cost for an ordinary domain is 1 credit, the old 6-credit figure is historical. Use the estimator for the product you will actually run.

👉 [Run your target URLs through ScraperAPI's plans and cost controls](https://www.scraperapi.com/pricing/?fp_ref=coupons)

## Failed requests still consume credits

Both providers bill some failures, and the definitions differ enough to change your real cost per usable record.

ScraperAPI charges for 200 and 404 responses, and for requests you cancel before giving it the recommended 70 seconds to finish. A page that returns a CAPTCHA shell with a 200 status is billable and useless.

Scrape.do bills 2XX plus target-side 400, 404 and 410 responses. Concurrency 429, provider 502 and client-cancelled 510 are listed as non-billable, which matters if your crawler is aggressive enough to hit rate limits.

The practical takeaway: stop comparing cost per 1,000 requests. Compare cost per 1,000 usable records, and include the retries. A cheap request that returns an empty product page and has to be re-run three times is not cheap.

## The $29 and $49 plans both fail the "global plus residential" test

This is where most comparison articles stop at the sticker price and miss the actual gate.

ScraperAPI restricts Hobby and Startup to US and EU geotargeting. Global `country_code` starts at Business.

Scrape.do advertises 160+ countries, but its own migration documentation says `super=true`, the residential and mobile proxy pool, requires the Business plan at $249. Datacenter geotargeting via `geoCode` requires Pro at $99. Postal code targeting needs `super` plus `geoCode` plus one of 12 supported countries.

So neither entry plan sells you "global plus residential." At $29 you get global datacenter routing only from Pro upward. At $49 you get US/EU only, residential or not. Budget for the tier that actually matches your target list, not the tier on the ads.

## Features that decide migration cost

ScraperAPI's structured-data layer covers Amazon, Google, Walmart, eBay and Redfin, with 18 documented endpoints plus `autoparse`. The async service at `async.scraperapi.com` accepts up to 50,000 URLs per `/batchjobs` submission, retries for 24 hours, and keeps results for 72 hours with 24 guaranteed. Sessions use `session_number` with a 15-minute TTL. There is `max_cost=50` to cap a single request's spend, `wait_for_selector`, `device_type=mobile|desktop`, `follow_redirect=false` and `keep_headers=true`, plus DataPipeline for scheduled no-code jobs with webhook delivery.

Scrape.do's equivalent is plugin endpoints: `/plugin/amazon/pdp` for product JSON, `/plugin/google/search` at 10 credits covering AI Mode and 84 Google domains, with Maps, Shopping, Flights, Hotels, News and Trends alongside. Its async queue at `q.scrape.do` runs on a separate concurrency pool of 30% of your plan limit, so batch jobs do not starve your synchronous traffic. It also has tablet emulation, exact viewport control, WebSocket message capture, custom JavaScript via an `Execute` action, and page-load control through `waitUntil=networkidle0|networkidle2|load`.

Migrating in either direction has two easy-to-miss unit changes. ScraperAPI's browser instruction `{"type":"wait","value":2}` is in seconds; Scrape.do's `{"Action":"Wait","Timeout":2000}` is in milliseconds, so multiply by 1,000. And Scrape.do's `sessionId` expires after 5 minutes without a request, while ScraperAPI's session window is 15 minutes. Any login-state workflow built around the longer window needs recalculating.

ScraperAPI has no tablet emulation, viewport control, WebSocket capture or custom `Execute` JS. Scrape.do has no `max_cost` equivalent and no direct `ultra_premium` tier. Both gaps are documented, not rumours.

## What the published speed and success numbers prove

The most quoted figures come from a benchmark Scrape.do ran itself across Amazon, Indeed, GitHub, Zillow, Capterra, Google and X: ScraperAPI at 15.7 seconds average, 92.70% success and $8.49 per 1,000 requests, against Scrape.do at 4.7 seconds, 98.19% and $0.80. Scrape.do publishes those numbers on its own site and in its own repository, so they describe Scrape.do's test conditions, not a neutral verdict.

An independent write-up published in October 2026 is more careful, and more useful: it states there is no defensible universal speed winner because nobody has run both providers against the same URL list, country, mode and parser configuration. It also notes that the two companies define uptime and success differently, so the figures cannot be swapped for one another. Vendor uptime is service availability, not the share of pages that came back with usable data.

The honest reading: Scrape.do likely has an edge on speed and on Google SERPs, and its own benchmark is the source for that claim. ScraperAPI's structured endpoints are strong on e-commerce and real estate, which the same competitor writeup concedes. Neither claim justifies picking a provider before you test your own 50 URLs.

On reviews, the Capterra sub-scores circulating for ScraperAPI are 4.9/5 for ease of use, 4.6/5 for customer service, and 4.5/5 each for features and value. Those come via a third-party summary rather than a direct read of the Capterra page, so treat them as directional. The recurring theme in them is that integration is painless because the API can be dropped in as a proxy replacement.

## Which one to buy

Plain pages, low volume, tight budget: Scrape.do Hobby at $29 for 250,000 credits, with the caveat that residential routing requires Business at $249. If your targets are ordinary HTML and datacenter IPs are fine, you will not notice.

Parsed product or SERP data, scheduled jobs, and a bill you need to cap: ScraperAPI Hobby at $49. The 18 structured endpoints, 50,000-URL batch queue, DataPipeline and `max_cost` replace code you would otherwise write and maintain. Paying $20 more for 150,000 fewer credits is a bad trade on paper and a good one when infrastructure time costs more than the subscription.

Global geotargeting plus residential IPs: neither entry plan. ScraperAPI needs Business, Scrape.do needs Business at $249. Price that tier before you commit to either vendor.

Already running one and considering a switch: the API surfaces are similar enough that proxy-style calls port over in an afternoon. The work is in the details, the seconds-versus-milliseconds wait unit, the session TTL, the credit multiplier differences on your top domains, and whichever structured endpoint you depend on.

## Before you pay

Check the signup balance against the marketing copy. ScraperAPI's trial and its FAQ describe different free allowances, and only the dashboard shows what you actually got.

Run your ten most important URLs through the cost estimator or the API playground, with the parameters you will use in production, and read the `sa-credit-cost` response header. That header tells you what each request really cost, which is more reliable than any per-1K figure on a comparison table.

👉 [Check ScraperAPI's current Hobby pricing and start with the free credits](https://www.scraperapi.com/pricing/?fp_ref=coupons)
