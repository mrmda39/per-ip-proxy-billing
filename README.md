# pay per ip proxies: how per-IP billing actually works, what it costs, and when it beats paying by the gigabyte

A per-GB proxy bill is a bet you make before you know the size of the pages you're going to load. Buy 50 GB for a scraping project and you might finish the month with 30 GB left, or you might burn through it in nine days because the target site started shipping 3 MB of JavaScript per page. Pay-per-IP pricing exists to remove that variable. You buy a fixed number of IPs instead of a fixed number of gigabytes, and the traffic meter disappears.

That's the whole idea. The details are where it gets interesting, because "pay per IP" sounds simpler than it is, and a few of the constraints only show up after you've paid. This piece walks through how the model works in practice, what per-IP capacity currently costs across the tiers published by one of the providers built around it — 9Proxy — and the checks worth running before you commit money.

## What "pay per IP" actually means

In an IP-based plan, you purchase a block of residential IPs. Each one forwards traffic for as long as it stays online, with no data cap attached. You are billed once for the IP, not per megabyte that passes through it.

Two mechanics usually get buried in the FAQ:

**An IP's lifespan is not yours to decide.** Residential IPs belong to real households. When the person at that address reboots their router or their ISP reassigns the address, the IP goes away. In 9Proxy's case, the company's own documentation puts typical IP duration between a few hours and roughly 24 hours, varying per IP. Some IPs die after 40 minutes. That's not a malfunction; it's what residential networks do.

**"One IP = one usage."** In 9Proxy's documentation, the generation logic is explicit: 1 IP = 1 usage when forwarded. Generate an endpoint and route traffic through it, and that IP is drawn from your balance. What does *not* expire is an IP you never used — unused IPs stay in your account indefinitely, which is a meaningful difference from subscriptions that reset every month.

So the honest framing is that you're buying IP sessions, not IPs you own. That distinction decides whether the model suits your project at all.

## Why the billing model matters more than the headline rate

Proxy pricing pages love a "from $0.68/GB" banner. Run the numbers on a real workload and the useful comparison becomes obvious.

A 2026 write-up on optimising scraping costs with Scrapeless estimated that a project processing 100,000 JavaScript-heavy pages a month — pages in the 2–5 MB range — could accumulate $1,500 to $3,000 in per-GB proxy fees. On a per-IP plan, the same 100,000 pages could run through a handful of IPs, and the bill wouldn't move when a site redesigns itself into a heavier front end.

The reverse case is just as real. If your workflow fires one 80 KB request per IP and needs maximum rotation, per-IP billing wastes money: you're buying whole sessions to fetch a rounding error of data. Per-GB pricing handles that better, and every serious provider publishes both.

The trick is knowing which shape your traffic is, which is usually one of these:

| Your workload | What the meter punishes | Better fit |
| --- | --- | --- |
| Long logged-in sessions, cart flows, account work | IPs dying mid-session, not bandwidth | Per-IP, unlimited bandwidth |
| Full-page rendering, media-heavy pages, large API responses | Total gigabytes | Per-IP |
| High-rotation SERP checks, price polling, light requests | Paying for sessions you barely use | Per-GB |
| Mixed projects with separate budgets per client | Rigid single-model plans | IP + GB bundles |

## How 9Proxy structures the per-IP side

9Proxy is a residential proxy platform selling two models side by side, which makes it a useful case study for anyone shopping by billing type. Its network is advertised as 20M+ residential IPs across 90+ countries, with targeting that goes down to country, city, ZIP code and ISP level, and a claimed 99.95% uptime. Those are vendor figures, not audited ones.

👉 [See the current per-IP packages and what each tier costs](https://bit.ly/9-Proxy)

The IP-based product works like this:

- **Unlimited bandwidth per IP.** No data cap during the IP's active life. A 10 MB page and a 100 KB page cost the same.
- **Unused IPs never expire.** No monthly burn of IPs you didn't touch.
- **Desktop app required.** IP-based plans route through the 9Proxy app using local port forwarding, with optional proxy authentication. You can't run them straight from the dashboard the way GB-based plans work.
- **Rotation is manual by design.** There's no natural rotation on IP plans. If you want an IP to change on a schedule, you use the Auto Rotation Proxy feature and set intervals per port.
- **Auto-Refresh Proxy.** When an IP drops offline, the system detects and swaps it, and 9Proxy claims a replacement window measured in tens of seconds rather than hours.
- **The Today List.** IPs from the previous 24 hours can be reused for free, which the vendor and third-party write-ups both describe as cutting IP consumption by roughly 20–30% on repeat tasks.
- **Protocols and integrations.** HTTP, HTTPS and SOCKS5, which covers anti-detect browsers like Multilogin, AdsPower and Dolphin Anty, plus Scrapy, Puppeteer and similar tooling.

One operational note worth flagging early: on IP-based plans, the desktop app is mandatory, and the model has been reported to no longer support media streaming (YouTube-style traffic) under an updated acceptable use policy. If streaming is anywhere in your plan, confirm the current terms before you pay.

## The full plan lineup

Per-IP pricing only makes sense against the alternatives, so here's the complete set of packages 9Proxy currently advertises across its three product types. IP prices were adjusted during 2026 — older reviews still quote the previous rates, including $20 for 100 IPs — so treat the numbers below as list prices to confirm at checkout rather than permanent facts.

**Residential proxy by IPs (unlimited bandwidth per IP)**

| Package | What you get | Published price | Effective rate | Buy |
| --- | --- | --- | --- | --- |
| 100 IPs | Entry block, unlimited bandwidth | $24 | ~$0.24/IP | [Start with the 100 IP package](https://bit.ly/9-Proxy) |
| 500 IPs | 5x entry block | $72 | ~$0.14/IP | [Check the 500 IP package](https://bit.ly/9-Proxy) |
| 1,000 IPs + 500 bonus | 1,500 usable IPs | $126 | ~$0.08/IP | [See the 1,000 + 500 bonus tier](https://bit.ly/9-Proxy) |
| 2,500 IPs | Mid-tier block | $210 | ~$0.08/IP | [Compare the 2,500 IP tier](https://bit.ly/9-Proxy) |
| 5,000 IPs | Agency-scale block | $360 | ~$0.07/IP | [View the 5,000 IP package](https://bit.ly/9-Proxy) |
| 15,000 IPs | Regional team scale | $720 | ~$0.05/IP | [Check the 15,000 IP package](https://bit.ly/9-Proxy) |
| 25,000 IPs | Reseller entry | $863 | ~$0.035/IP | [See the 25,000 IP tier](https://bit.ly/9-Proxy) |
| 50,000 IPs | High-volume reseller | $1,438 | ~$0.029/IP | [View the 50,000 IP package](https://bit.ly/9-Proxy) |
| 100,000 IPs (Business) | Enterprise block | $2,300 | ~$0.023/IP | [Ask about the 100,000 IP business package](https://bit.ly/9-Proxy) |
| 200,000 IPs (Business) | Enterprise block | $4,140 | ~$0.021/IP | [Check business volume pricing](https://bit.ly/9-Proxy) |
| 500,000 IPs (Business) | Enterprise block | $8,625 | ~$0.018/IP | [See the 500,000 IP enterprise tier](https://bit.ly/9-Proxy) |

**Residential proxy by GB (rotating or sticky sessions)**

| Package | Validity | Published price | Effective rate | Buy |
| --- | --- | --- | --- | --- |
| 5 GB | 180 days | $15 | $3.00/GB | [Try the smallest GB package](https://bit.ly/9-Proxy) |
| 50 GB + 5 GB bonus | 180 days | $105 | ~$2.10/GB | [Check the 50 GB package](https://bit.ly/9-Proxy) |
| 100 GB | 180 days | $150 | $1.50/GB | [See the 100 GB tier](https://bit.ly/9-Proxy) |
| 200 GB | 180 days | $200 | $1.00/GB | [View the 200 GB package](https://bit.ly/9-Proxy) |
| 1,000 GB | 180 days | $800 | $0.80/GB | [Compare the 1,000 GB tier](https://bit.ly/9-Proxy) |
| 2,000 GB | 180 days | $1,500 | $0.75/GB | [Check the 2,000 GB package](https://bit.ly/9-Proxy) |

**IP + GB bundles** (combining both meters in one package, with bundled traffic valid 180 days)

| Package | Contents | Published price | Buy |
| --- | --- | --- | --- |
| Starter Bundle | 100 IPs + 5 GB | $25 (listed as $10 off $35) | [Look at the Starter bundle](https://bit.ly/9-Proxy) |
| Growth Bundle | 1,500 IPs + 50 GB | $150 (listed as $60 off $210) | [See the Growth bundle](https://bit.ly/9-Proxy) |
| Pro Bundle | 5,000 IPs + 500 GB | $600 (listed as $250 off $850) | [Compare the Pro bundle](https://bit.ly/9-Proxy) |

Nothing on that list is a subscription. There's no auto-renewing monthly commitment in the published structure — you buy a package, spend it, top up when you need more. That's the main structural difference from ISP proxy plans at larger vendors, which often bill per IP per month.

## The fine print that decides whether per-IP pays off

Four things separate a good per-IP experience from an expensive one, and they're all checkable before purchase.

**The invalid-IP credit window.** 9Proxy's published terms are narrow: credits essentially cover IPs that die within about 60 seconds of being forwarded. An IP that works for two hours and then drops is a normal residential event, not a refund case. If your workflow can't tolerate that, budget for replacement IPs rather than assuming you'll get them back.

**Session stability versus account safety.** The most common complaint pattern attached to 9Proxy's Trustpilot profile is exactly this: IPs going offline roughly an hour in, sometimes triggering security flags on the accounts being managed. The company's public response has been consistent — that's the nature of shared dynamic residential pools, and workloads needing a fixed address should be on static ISP proxies instead. That's a fair technical answer, and it's also the honest reason not to use a rotating residential pool for anything that treats a stable IP as a security signal.

**Geography depth.** Country-level targeting is table stakes. City, ZIP and ISP targeting is what makes residential IPs worth the premium for ad verification, local SERP checks and regional pricing work. Confirm the exact locations you need exist in the pool before buying in bulk.

**Acceptable use.** Sneaker copping, gaming and streaming are either restricted or excluded from IP-based plans. Buying 5,000 IPs for a use case that violates the terms gets you a suspended account, not a refund.

## Who should buy per-IP proxies, and who shouldn't

Per-IP wins when bandwidth is the unpredictable variable and the IP count is knowable. Price aggregators, SERP monitoring on heavy pages, market research across regions, and multi-account setups run through anti-detect browsers all fit — you can pre-budget the month with arithmetic instead of guesswork.

It loses when rotation is constant and payloads are tiny. API polling, lightweight geo-checks and one-request-per-IP scraping burn sessions for nothing. That's what the GB plans are for, and 9Proxy sells both for that reason.

It also loses when you need a genuinely permanent address. Nothing in a shared residential pool provides that, at 9Proxy or anywhere else. Static ISP proxies are the correct product there, and 9Proxy's support has said as much publicly.

## Common questions

**Is per-IP cheaper than per-GB?** Only relative to your traffic shape. At the entry tier, 100 IPs at $24 works out to roughly $0.24 per session — but each of those sessions carries unlimited data. Push 100 GB through those same 100 IPs and per-GB billing at $1.50/GB would have cost $150. The crossover point sits far below what most heavy-page scrapers actually consume.

**Is there a free trial?** Answers differ across sources. Review directories list a trial as available with no card required; 9Proxy's own public answers describe a limited trial for new users subject to availability, with IP-based and GB-based trials offered separately. The safest read is that a trial exists but isn't guaranteed, so ask support which one you want rather than assuming.

**Is 9Proxy legitimate?** It's been operating since 2023, publishes full technical documentation, responds to negative reviews publicly, and appears in third-party comparisons with roughly 3.9/5 editorial scores. Its Trustpilot aggregate is considerably harsher, driven mostly by refund-policy friction and IP stability complaints from buyers who picked the wrong billing model for their workload. Both of those things can be true at once.

**What happens to IP purchases I haven't used?** They don't expire. That's a real advantage over monthly plans — buy 5,000 IPs in a slow month and the unused balance is still there when the next project lands.

👉 [Check the current per-IP rates and pick the tier that matches your workload](https://bit.ly/9-Proxy)

The short version: per-IP pricing removes bandwidth anxiety and replaces it with session-lifespan management. If your projects are bandwidth-heavy and your IP needs are countable, that's a good trade. If they're bandwidth-light and rotation-heavy, stay on GB billing and don't overthink it.
