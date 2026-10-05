# proxies for web scraping: how to pick rotating vs sticky sessions, calculate cost per 1,000 pages, and start from a $5 test pack

Most scrapers don't fail because of a bad CSS selector. They fail because the target site starts answering with 403s, CAPTCHAs, or a challenge page that returns HTTP 200 and no data. That's a proxy problem, and it's where most scraping budget quietly disappears.

So before you compare provider logos, it helps to know what you're actually buying: IP type, session behaviour, and a price unit that hides your real costs. Below is how the pieces fit together, and how DataImpulse's pay-as-you-go model sizes up against that checklist.

## Rotating vs sticky: the setting that decides your block rate

A rotating session hands you a fresh exit IP on every request. A sticky session pins one IP to one port for a set window. DataImpulse supports both, and the switch lives in the proxy username rather than in the dashboard, so you can change behaviour per request without re-provisioning anything.

The rotating endpoints are straightforward:

- `gw.dataimpulse.com:823` for HTTP/HTTPS
- `gw.dataimpulse.com:824` for SOCKS5

Sticky sessions use ports in the 10000–20000 range. You can request a rotation interval from 1 to 120 minutes, and if you leave the interval unset (or set it to 0), the default is 30 minutes.

There's a caveat worth internalising before you build a login flow on top of sticky sessions. Residential IPs come from real people's devices, and those devices go offline. DataImpulse support confirms the configurable maximum is 120 minutes but the realistic average is around 30, and that when the underlying device drops, the proxy rotates to the next available IP automatically. No provider that rents consumer IPs can guarantee a full 120 minutes, and any provider claiming otherwise is describing the setting, not the behaviour.

Which one to use:

| Task | Session type | Why |
| --- | --- | --- |
| Product listings, SERPs, broad crawls | Rotating | Every request is independent; spreading load avoids per-IP rate limits |
| Logged-in flows, checkouts, paginated results | Sticky | The target ties your session to the IP that started it |
| Price monitoring across regions | Rotating with country target | Fresh IP plus consistent geography |

Rotating is the default for scraping. Reach for sticky when a site breaks on IP changes mid-flow, not because it sounds more advanced.

## Datacenter, residential or mobile? Match the IP type to the target

Overspending on proxies is usually a type mismatch, not a pricing problem.

Datacenter IPs come from server ranges. They're fast, cheap, and easy to fingerprint, which means they get blocked fast on anything running Cloudflare, DataDome or PerimeterX. Use them for open APIs, public government datasets, and unprotected sites where you're pulling volume.

Residential IPs route through consumer ISPs, so requests look like ordinary home traffic. This is the default choice for e-commerce, travel, real estate, and SERP work. The trade-off is cost per GB and less predictable latency, because you're routing through someone's home router.

Mobile IPs sit on 4G/5G carrier networks. Carrier-grade NAT means many users share an address, which makes mobile IPs unusually hard to block and unusually expensive. Reserve them for mobile-only APIs, app data, and the targets that have already defeated everything else.

The practical rule: if a datacenter IP works on your target, keep using it. Paying $2/GB for mobile traffic to scrape a static HTML page is the most common budget leak in this space.

👉 [See DataImpulse's residential, datacenter and mobile options in one dashboard](https://bit.ly/dataimPulse)

## The pricing trap: $/GB is not the number you should compare

Proxy providers quote three different units: per gigabyte, per IP per month, and per 1,000 requests. They measure different things, so a side-by-side table of headline prices is mostly decoration. A $1/GB pool that bounces half your requests costs more per usable page than a $2/GB pool that returns nine out of ten.

Two hidden variables matter more than the sticker price:

**Traffic expiry.** If unused gigabytes reset every billing cycle, you're buying capacity you may never use. DataImpulse doesn't expire purchased traffic on any tier, which matters if your crawl calendar is lumpy — one heavy week, two quiet ones.

**Targeting surcharges.** Country targeting is included. State, city, ZIP and ASN selection are billed at double the standard per-GB rate on standard residential. Third-party analysis notes that the datacenter product page lists the same filters as included features, which is worth confirming with support before you build a budget around it.

### Worked example: what a million pages actually costs

Assume 1,000,000 HTML pages a month at roughly 0.5 MB each. That's about 500 GB of traffic. At $1/GB, that's $500/month. Buy the 1 TB tier at $0.80/GB and the same workload lands around $400.

Then adjust for reality. If nine in ten requests return usable HTML, divide by 0.9 — the same 500 GB now represents $556 of spend for 1,000,000 *usable* pages. And if you're rendering with Playwright or Puppeteer, remember that a headless browser pulls images, fonts and scripts, so your per-page byte count can be several times higher than raw HTML. Scrape a few thousand pages with your actual tooling before you commit to any volume tier.

## DataImpulse plan breakdown: four products, pay-as-you-go, no subscription

DataImpulse runs four proxy products on the same billing model: you buy traffic, it never expires, and there's no monthly plan to cancel. The minimum top-up is $5, and the $5 entry buys 5 GB of residential, 10 GB of datacenter, 2.5 GB of mobile, or 1 GB of premium residential.

Here's the full published ladder across all four products.

| Proxy type | Plan | Traffic | Price | Effective rate | Notes | Buy |
| --- | --- | --- | --- | --- | --- | --- |
| Residential | Intro | 5 GB | $5 | $1.00/GB | Cheapest real test of the pool; 7-day money-back on first card purchase | [Start with 5 GB](https://bit.ly/dataimPulse) |
| Residential | Basic | 50 GB | $50 | $1.00/GB | Same rate, bigger balance; 90M+ IP pool, 195 countries | [Get the 50 GB balance](https://bit.ly/dataimPulse) |
| Residential | Advanced | 1 TB | $800 | $0.80/GB | 20% volume discount plus a dedicated account manager | [See the 1 TB volume tier](https://bit.ly/dataimPulse) |
| Residential | Custom+ | 5 TB+ | From $4,000 | Custom | Personalised configuration | [Request custom residential pricing](https://bit.ly/dataimPulse) |
| Datacenter | Intro | 10 GB | $5 | $0.50/GB | 99.9% uptime, randomised datacenter subnets | [Start with 10 GB datacenter](https://bit.ly/dataimPulse) |
| Datacenter | Basic | 100 GB | $50 | $0.50/GB | Good fit for unprotected APIs and bulk public data | [Get 100 GB datacenter](https://bit.ly/dataimPulse) |
| Datacenter | Advanced | 1 TB | $450 | $0.45/GB | Volume tier for continuous crawls | [See the 1 TB datacenter tier](https://bit.ly/dataimPulse) |
| Datacenter | Custom | 5 TB+ | From $2,250 | Custom | Personalised configuration | [Request custom datacenter pricing](https://bit.ly/dataimPulse) |
| Mobile | Intro | 2.5 GB | $5 | $2.00/GB | 3G/4G/5G/LTE carrier IPs | [Start with 2.5 GB mobile](https://bit.ly/dataimPulse) |
| Mobile | Basic | 25 GB | $50 | $2.00/GB | Rotating and sticky sessions supported | [Get 25 GB mobile](https://bit.ly/dataimPulse) |
| Mobile | Advanced | 1 TB | $1,600 | $1.60/GB | Volume discount on mobile | [See the 1 TB mobile tier](https://bit.ly/dataimPulse) |
| Mobile | Custom | 5 TB+ | From $8,000 | Custom | Personalised configuration | [Request custom mobile pricing](https://bit.ly/dataimPulse) |
| Premium residential | Intro | 1 GB | $5 | $5.00/GB | High-speed pool, all targeting options included | [Start with 1 GB premium](https://bit.ly/dataimPulse) |
| Premium residential | Basic | 10 GB | $50 | $5.00/GB | Dedicated account manager | [Get 10 GB premium](https://bit.ly/dataimPulse) |
| Premium residential | Custom | 5 TB+ | From $20,000 | Custom | Enterprise terms, all targeting at no surcharge | [Request premium pricing](https://bit.ly/dataimPulse) |

Every product supports HTTP, HTTPS and SOCKS5, country-level targeting at no extra cost, and non-expiring traffic. Premium residential is the only tier where advanced targeting isn't billed as an add-on.

## What independent testing says

Vendor specs are marketing until someone else measures them. Three outside sources fill that gap.

Proxyway's annual market research runs millions of connection requests per provider over roughly three weeks. For DataImpulse it recorded a **99.51% success rate** and a **1.22-second response time**, with the advertised 90M IP pool described as small-to-average and coverage tested at 150+ locations. The same review flags two honest downsides: the price doubles if you need granular targeting, and the low rate means IPs can get abused by other customers, which shows up as more blocks on heavily targeted sites. That second point is the structural cost of cheap bandwidth, and it applies to every low-cost pool, not just this one.

ProxyStats benchmarks the network continuously with automated probes. Across a 30-day window covering 37,871 tests, it measured 93.5% success over 24 hours, 93.6% over 7 days and 93.7% over 30 days, with a median latency of 530 ms and a P95 of 1,600 ms. Read both sets of numbers together and the picture is consistent: performance is solid for mainstream scraping, and the gap between a published success rate and an independent one is exactly the gap between a lab test and your target list.

HostAdvice's hands-on review covers the operational side. The dashboard includes a proxy-list generator where you pick country, rotation behaviour, protocol and output format, plus a cURL string that updates as you change settings so you can sanity-check a connection before writing any code. Usage tracking breaks down spend, traffic and requests, and can drill into which sites were accessed and how much traffic each consumed in one-minute intervals. There's a REST API for standard users and resellers, documented on Postman with Python, Go and cURL examples. Their support test got a substantive technical answer on sticky-session duration in about seven minutes — from a human, not a bot loop.

## Setting it up: three lines and a sanity check

Get the credentials format right and the rest is ordinary proxy configuration. A rotating residential call looks like this:

python
import requests

proxy = "http://YOUR_LOGIN__cr.us:YOUR_PASSWORD@gw.dataimpulse.com:823"
proxies = {"http": proxy, "https": proxy}

r = requests.get("https://httpbin.org/ip", proxies=proxies, timeout=30)
print(r.text)


The `__cr.us` segment is the country target. For a sticky session you keep the same username pattern but move to a port in the 10000–20000 range and set the rotation interval you want.

Before scaling, measure four things against your actual targets: success rate, block rate, geo accuracy, and seconds per page. A pool that looks cheap on paper can lose that advantage the moment your success rate drops, and you won't see it until the invoice arrives.

## Who this fits, and who should look elsewhere

DataImpulse makes sense if your workloads are uneven and your targets aren't the hardest on the internet. Prepaid gigabytes that never expire solve a real problem for teams running bursty crawls, and $1/GB with a $5 entry point means testing the pool against your own target list costs less than lunch. Price monitoring, SERP collection, market research, and general e-commerce scraping fit the standard residential pool comfortably.

It's a worse fit in a few specific cases. There's no managed unblocking layer comparable to Bright Data's Web Unlocker, so if you're fighting Cloudflare or DataDome at scale, the TLS fingerprinting and retry logic are yours to handle. There's no standalone static ISP or dedicated-IP product, which matters if you need one stable address per account for marketplace or ad-platform management. And at very high concurrency on aggressive targets, a larger pool can still edge ahead — the 90M+ figure is mid-tier next to the 400M-class networks.

One structural trade-off to keep in mind: $5/GB premium residential exists precisely because the $1/GB pool is a shared, best-effort commodity. If your first tests show inconsistent results on one specific domain, that's the tier designed for you, not a reason to abandon the provider.

## FAQ

**Do you need residential proxies for web scraping?**
Only if your target blocks datacenter ranges. Public datasets, open APIs and unprotected pages are cheaper and faster over datacenter IPs at $0.50/GB. Test with a small datacenter pack first and move to residential when you see 403s.

**Rotating or sticky for a product-listing crawl?**
Rotating. Each request is independent, and spreading load across many IPs avoids the per-IP rate limits that kill single-address crawls.

**Does DataImpulse offer a free trial?**
No free tier. The entry point is a $5 pack — 5 GB residential, 10 GB datacenter, 2.5 GB mobile, or 1 GB premium residential. There's a 7-day money-back guarantee on first purchases paid by card, provided less than 80% of the traffic has been consumed. Crypto purchases are non-refundable.

**Does purchased traffic expire?**
No. Unused gigabytes stay in the account across all tiers, which is the main reason the pay-as-you-go model suits irregular crawl schedules.

**Is advanced geo-targeting included?**
Country selection and ASN exclusion are free. State, city, ZIP and ASN selection are billed at 2× the standard per-GB rate on standard residential plans, and included on premium residential. Datacenter listings suggest those filters are included — confirm current treatment before you build a budget around it.

**How long can a sticky session last?**
You can configure up to 120 minutes, with a 30-minute default. Expect the realistic average to land near 30 minutes, because residential IPs depend on a real device staying online.

If your current scrape is burning bandwidth on challenge pages and 403s, the fastest way to find out whether a different pool fixes it is to run a few thousand requests of your own against your actual targets.

👉 [Start with the $5 DataImpulse pack and test it against your own target list](https://bit.ly/dataimPulse)
