# Backconnect residential proxies: what one rotating endpoint replaces, and how per-IP vs per-GB pricing plays out in practice

If you've ever pasted 800 lines of `ip:port:user:pass` into a scraper and watched half of them die before lunch, you already know why people search for backconnect residential proxies. The pitch is that you stop managing a list: one address handles the routing, and the pool behind it decides which residential IP your request leaves from.

What trips people up is the billing. Two providers can both advertise "residential proxies" and charge in completely different units, per gigabyte or per IP, and the one that looks cheaper on the pricing page is often the more expensive choice for your specific workload. That's the decision worth getting right before you buy anything, so let's work through it with a concrete provider as the example: 9Proxy, which currently sells both models side by side.

## What "backconnect" actually means

A backconnect proxy is a single gateway sitting in front of a pool of exit IPs. You authenticate to one host, and the gateway assigns your request to a residential IP from the pool — a fresh one per request, or the same one for a set window if you ask for a sticky session. Rotation happens server-side. Your script only ever knows one endpoint.

The reason the architecture exists is rate limiting and IP reputation. Send a few thousand requests from one address and you'll get blocked, throttled, silently served wrong data, or hit with a CAPTCHA wall. Spread those requests across thousands of residential IPs and each one looks like an ordinary household connection from wherever you aimed it.

Terminology drifts here, so it's worth being precise. Some vendors reserve "backconnect" for residential pools, some use it for mobile, and plenty use it interchangeably with "rotating proxies." What it isn't is a reverse proxy, which sits in front of a server to protect that server and keeps a fixed address.

## Rotating or sticky: the choice that decides everything downstream

Both modes use the same pool and the same endpoint. They behave very differently.

**Rotating** gives you a new IP on every request, or at a short interval. It's the right mode for volume jobs where no single request depends on the last one: SERP checks, price scraping, ad verification, link auditing, anything where you're collecting independent data points across locations.

**Sticky** holds the same IP for a configured window, typically minutes. You need it the moment a workflow has state — logging in, filling a multi-step form, keeping a cart, running a social account through several pages. Rotate the IP mid-session on those and you'll get logged out, flagged, or both.

Getting this wrong is the single most common way people conclude that "residential proxies don't work." The pool was fine. The session mode wasn't.

## How 9Proxy splits its residential product

9Proxy runs two separate residential models, and the split maps neatly onto the rotating-versus-sticky question.

**Residential by IP** gives you a fixed number of individual residential IPs with unlimited bandwidth on each. The IPs stay live from a few hours up to roughly 24 hours, unused IPs don't expire, and there's no traffic cap while an IP is active. Rotation isn't the default behaviour here — you get it through an auto-rotation proxy that switches IPs at intervals you set on selected ports. It's built for session-stable work and heavy transfer where bandwidth is hard to predict.

**Residential by GB** is the classic backconnect setup for this keyword. You pay for traffic instead of addresses, generate unlimited endpoints from the dashboard, and pick rotating or sticky mode yourself. Authentication is either username/password or IP whitelisting, and the traffic stays valid for 180 days, which matters if your projects come in bursts rather than running monthly.

Targeting on the GB side goes down to country, state, city, ZIP and ISP level, which is the level of granularity you actually need for local rank tracking or verifying what a specific metro sees. Both HTTP/HTTPS and SOCKS5 are supported, so Scrapy, Playwright, Puppeteer and anti-detect browsers can point at the same endpoint without protocol gymnastics.

## Where backconnect residential proxies earn their cost

The use cases are broader than the scraping default people assume. A short list of jobs where a rotating residential endpoint fixes a problem that a datacenter IP can't:

- **Price and stock monitoring** across regional storefronts, where a foreign datacenter IP gets served a different catalogue or a blocked page.
- **SERP and rank tracking** by city, since search results genuinely differ by metro in a lot of verticals.
- **Ad verification**, checking what a campaign actually serves to real users in a target market rather than what the ad platform reports.
- **Multi-account operations**, where sticky sessions keep each identity on a consistent IP for the length of a working session.
- **Security testing and OSINT**, where your own IP is part of the traffic profile and a flagged test IP invalidates the exercise before it reaches the application layer.

If your target is a lightly protected site and you just need throughput, a datacenter proxy is cheaper and faster. Backconnect residential IPs are the answer when the target treats IP reputation as a signal.

## The pricing maths: per GB or per IP

Two units, two kinds of workload, and the arithmetic is different for each.

**Pay per GB** when your jobs rotate aggressively and each request moves little data. A SERP check, an API poll, a price read — those consume kilobytes. You're buying the right to spread requests across the whole pool without counting addresses, so heavy rotation is effectively free; only bandwidth is metered. 9Proxy's published GB tiers run from $3.00/GB at the 5 GB entry package down to $0.75/GB at 2,000 GB, with the company's headline "from $0.68/GB" rate appearing at the 10,000 GB enterprise tier.

**Pay per IP** when sessions have to last and bandwidth is unpredictable. If you're pulling large volumes of page data, watching video metadata, or running long authenticated sessions, per-IP with unlimited bandwidth stops the meter. At 9Proxy, 100 IPs cost $24 and the rate falls steeply from there — $0.084/IP at the 1,000+500 bonus tier, $0.029/IP at 50,000, and $0.018/IP at the 500,000 business tier.

There's a third pattern worth checking before you buy either: bundles. If you need both stable IPs and flexible bandwidth, the combined packages are usually cheaper than the sum of the parts.

- The 100 IPs + 5 GB bundle is $30. Buying the same 100 IPs at $24 plus 5 GB at the $3.00/GB entry rate ($15) comes to $39, so the bundle saves you about $9.
- The 1,500 IPs + 50 GB bundle is $180. The 50 GB alone lists at $105, so the extra 1,500 IPs effectively cost $75.
- The 5,000 IPs + 500 GB bundle is $720. The IPs alone are $360, which means the 500 GB works out to $0.72/GB — lower than the cheapest published self-serve GB tier of $0.75/GB.

One timing detail: on 1 June 2026, 9Proxy raised IP-based and bundle pricing for the first time since launch, while leaving GB pricing untouched. If you find comparison pages quoting $20 for 100 IPs or $0.015/IP, those numbers predate the adjustment.

## Every 9Proxy plan currently on the price list

The tables below cover the full published line-up: IP-based packages including the high-volume business tiers, GB-based packages including the enterprise tier, and the bundles. Unused IP balances don't expire; GB traffic carries 180-day validity unless you're on the enterprise programme.

### Residential by IP (unlimited bandwidth per IP)

| Package | What you get | Price | Effective rate | Get it |
| --- | --- | --- | --- | --- |
| 100 IPs | 100 residential IPs, unlimited bandwidth | $24 | $0.24/IP | [Check current pricing and availability](https://bit.ly/9-Proxy) |
| 500 IPs | 500 residential IPs, unlimited bandwidth | $72 | $0.144/IP | [View the 500 IP package](https://bit.ly/9-Proxy) |
| 1,000 + 500 bonus IPs | 1,500 residential IPs in total | $126 | $0.084/IP | [See the bonus IP deal](https://bit.ly/9-Proxy) |
| 2,500 IPs | 2,500 residential IPs | $210 | $0.084/IP | [Open the 2,500 IP tier](https://bit.ly/9-Proxy) |
| 5,000 IPs | 5,000 residential IPs | $360 | $0.072/IP | [Get the 5,000 IP package](https://bit.ly/9-Proxy) |
| 15,000 IPs | 15,000 residential IPs | $720 | $0.048/IP | [Check the 15,000 IP rate](https://bit.ly/9-Proxy) |
| 25,000 IPs | 25,000 residential IPs | $863 | $0.035/IP | [View the 25,000 IP tier](https://bit.ly/9-Proxy) |
| 50,000 IPs | 50,000 residential IPs | $1,438 | $0.029/IP | [See the 50,000 IP package](https://bit.ly/9-Proxy) |
| Business 100,000 IPs | High-volume reseller tier | $2,300 | $0.023/IP | [Ask about the 100,000 IP plan](https://bit.ly/9-Proxy) |
| Business 200,000 IPs | High-volume reseller tier | $4,140 | $0.021/IP | [Check the 200,000 IP plan](https://bit.ly/9-Proxy) |
| Business 500,000 IPs | High-volume reseller tier | $8,625 | $0.018/IP | [See the 500,000 IP plan](https://bit.ly/9-Proxy) |

### Residential by GB (rotating or sticky backconnect endpoints)

| Package | What you get | Price | Effective rate | Validity | Get it |
| --- | --- | --- | --- | --- | --- |
| 5 GB | Unlimited endpoints, rotating or sticky | $15 | $3.00/GB | 180 days | [Start with the 5 GB package](https://bit.ly/9-Proxy) |
| 50 GB + 5 GB bonus | 55 GB of rotating traffic | $105 | $2.10/GB | 180 days | [Get the 50 GB package plus bonus](https://bit.ly/9-Proxy) |
| 100 GB | Rotating or sticky, unlimited endpoints | $150 | $1.50/GB | 180 days | [View the 100 GB plan](https://bit.ly/9-Proxy) |
| 200 GB | Rotating or sticky, unlimited endpoints | $200 | $1.00/GB | 180 days | [Check the 200 GB plan](https://bit.ly/9-Proxy) |
| 1,000 GB | Rotating or sticky, unlimited endpoints | $800 | $0.80/GB | 180 days | [See the 1,000 GB tier](https://bit.ly/9-Proxy) |
| 2,000 GB | Rotating or sticky, unlimited endpoints | $1,500 | $0.75/GB | 180 days | [Open the 2,000 GB tier](https://bit.ly/9-Proxy) |
| Enterprise (10,000 GB tier) | Unlimited validity, team seats, VIP pricing | Quoted per account | $0.68/GB at this tier | No expiry | [Request enterprise pricing](https://bit.ly/9-Proxy) |

### Bundles (IPs plus bandwidth)

| Package | What you get | Price | What it works out to | Get it |
| --- | --- | --- | --- | --- |
| Starter bundle | 100 IPs + 5 GB | $30 | ≈$9 less than buying both separately | [Pick up the Starter bundle](https://bit.ly/9-Proxy) |
| Popular bundle | 1,500 IPs + 50 GB | $180 | The 50 GB alone lists at $105, so the IPs add $75 | [Get the Popular bundle](https://bit.ly/9-Proxy) |
| Pro bundle | 5,000 IPs + 500 GB | $720 | 500 GB at $0.72/GB, below the cheapest standalone GB tier | [Go with the Pro bundle](https://bit.ly/9-Proxy) |

The enterprise programme sits on top of the GB product and adds features rather than bandwidth: unlimited data validity, a team mode with one owner and up to five members, no-expiration sharing inside the team, per-member traffic controls, activity logs and VIP pricing with dedicated support.

## Setting up a backconnect endpoint

The GB product runs entirely in the dashboard, no local install required. The flow is short:

1. Sign up and fund the account, or start on IP-based and download the desktop app if your traffic needs OS-level routing.
2. Open the Proxy Generator and set your targeting — country, state, city, ZIP or ISP.
3. Choose rotating or sticky. If sticky, set the session length you need.
4. Pick the authentication method: username/password, or whitelist your device IP and drop credentials entirely.
5. Export the endpoint list as .txt or .csv, or copy it and use the ready-made code samples for your language.

From there the endpoint drops into whatever you already use: SOCKS5 into Multilogin or similar anti-detect browsers, HTTP into rank trackers, or straight into Python requests and Scrapy. There's also Proxy2Web, a browser-based tool for checking what an endpoint actually looks like to a target site without running a script, and a public API if you'd rather generate and rotate proxies programmatically than click through a dashboard.

Payments are wide, including cards, Apple Pay, Google Pay, Alipay and crypto. Crypto payments come with an automatic 5% IP bonus, which is unusual enough to be worth knowing about if you already hold stablecoins.

## The limitations worth knowing before you buy

A few honest caveats, since none of this is free of trade-offs.

**The pool numbers you see advertised vary widely.** Vendor material and most independent reviews from 2025–2026 converge on roughly 20 million residential IPs across 90+ countries, but promotional pages sometimes claim far larger figures. Treat anything dramatically higher as marketing.

**Performance figures are vendor-published.** 9Proxy advertises around 99.5% success rate, ~0.6s average response time and 99.95% uptime. Independent benchmark summaries from proxy directories place rotating residential success around 97% with P95 latency near 1.3s — solid for a budget provider, not class-leading. Benchmark both numbers on your own targets before you commit volume.

**Refunds are credit-based, not cash.** Published terms replace an IP that dies within about 60 seconds of allocation automatically. That's automatic and narrow. There's no monetary refund path for "it didn't suit my use case," and there's no standard advertised free trial — trial access exists but is limited and granted at the provider's discretion, so ask before assuming.

**It's a residential-only shop.** No datacenter, ISP or mobile lines. If your workflow needs cheap datacenter IPs for low-protection targets, you'll be running a second provider alongside it.

**Streaming is off the table.** A 2026 update to the acceptable use policy removed media streaming support on IP-based plans. If YouTube or similar is your use case, confirm current terms first.

**It's a single provider, so it's a single point of failure.** In mid-2026 the service went through a widely reported multi-day disruption starting around 28 June, with the site, app and existing balances affected; the company acknowledged an outage but never published a root cause. Community reports from July describe the site back online with new purchases paused while existing customers were prioritised, and later comparison write-ups treat the service as operational again. Contrary to one competitor blog's framing, there's no credible evidence of any regulatory seizure. The takeaway isn't to avoid the platform — it's that prepaid proxy balances across any single vendor deserve a backup plan.

## Which plan fits which job

A short decision path, based on the numbers above:

- **Rotating-heavy, low data per request** (SERP checks, ad verification, API polling): buy GB. The 200 GB tier at $1.00/GB or the 1,000 GB tier at $0.80/GB is where the curve gets reasonable.
- **Long sessions and unpredictable bandwidth** (scraping large pages, authenticated workflows): buy IPs. The 1,000 + 500 bonus tier at $0.084/IP is the usual starting point for solo operators and small teams.
- **Both at once, or agency work with mixed clients**: take a bundle. The Pro bundle's 500 GB at an effective $0.72/GB alongside 5,000 IPs is the best value line in the table.
- **Team sharing and permanent traffic validity**: the enterprise route, quoted per account.

Small entry packages exist for testing a specific target before you commit to volume, and since unused IP balances don't expire and GB traffic lasts 180 days, an exploratory purchase isn't money you have to burn quickly.

👉 [Compare all 9Proxy residential plans and pick your package](https://bit.ly/9-Proxy)

The short version: backconnect residential proxies solve an IP-reputation problem, not a speed problem, and the billing unit you pick matters more than the brand on the invoice. Match the unit to how your workload behaves — rotation-heavy to GB, session-heavy to IP — and the pricing stops being a mystery.
