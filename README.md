# vps with dedicated ip: How to Spot a Real Dedicated IPv4 (Not NAT), What It Should Cost, and Which Plans Fit Mail Servers and VPNs

A listing that promises a "VPS with dedicated IP" can describe three very different products, and only one of them does what most people searching this term actually need: a public IPv4 assigned to your server alone, reachable on any port, with a reputation nobody else can damage. The short version: real dedicated-IP VPSes start at $11.29 per year at RackNerd (annual KVM specials), $2.48 per month at Cloudzy, and $5.49 per month at TruoCloud. The "$2 VPS" deals at the very bottom of the market are usually NAT, and NAT breaks mail servers, VPN endpoints, and anything that needs port 443.

> **Quick pick:** for most people, RackNerd's 1 GB KVM plan at **$11.29/year** (1 vCPU, 20 GB SSD, 1 TB of traffic on a 1 Gbps port, dedicated IPv4 included) is the value pick in this comparison. It works out to roughly $0.94 a month — less than half of what the monthly-billing hosts charge for plans with *less* RAM. Running a panel or several services? Step up to the 2 GB tier at $18.99/year. 👉 [Check RackNerd's current annual KVM specials](https://bit.ly/my-rn)

## What "dedicated IP" actually means on a VPS listing

Three arrangements exist in the budget VPS market, and hosts are often vague about which one you're buying. Only the first is a normal server.

| Setup | What you're really getting | Practical effect |
| --- | --- | --- |
| Dedicated IPv4 | One public IPv4 assigned to your VM alone | Bind any port, point a domain at it, issue TLS certs, build your own sending reputation |
| Shared IPv4 | One address in front of many customers, routed by a reverse proxy | Fine for plain websites, useless for non-HTTP services; one spamming neighbor hurts everyone |
| NAT VPS | A private address plus a handful of forwarded ports on a shared IP | No standard ports, no clean TLS; VPNs and self-hosting are painful or impossible |

Here's the heuristic worth remembering: IPv4 addresses cost providers real money — they're a scarce, traded resource — so nobody hands out a dedicated one by accident. If a listing doesn't explicitly say "dedicated IPv4" and the price sits far below the market, assume NAT until proven otherwise.

One misconception to clear up while we're here: a dedicated IP is exclusive, not private. It's public by definition, it geolocates to your datacenter, and it identifies your server. And because IPv4 addresses get recycled between customers, the fresh IP you're assigned may carry history you didn't write. Both points matter more than they sound — the next section covers what to actually do about them.

As for why people search for this in the first place, the usual drivers are practical: mail servers need an IP with a matching PTR record and no shared spam history; third-party APIs and corporate firewalls treat a static source IP differently, with higher rate limits and simple allowlisting; a VPN endpoint on a fixed address gets whitelisted at the office once and never touched again; monitoring probes need a predictable place to live.

## How to verify you're actually getting a dedicated IP

Two checks before you pay, two after deployment. None of them takes more than a couple of minutes.

Before ordering:

1. **Ask the pre-sales question directly:** "Is this a dedicated IPv4 or NAT? Can I bind arbitrary ports, including 25 and 443?" Reputable hosts answer in one sentence. Evasion is your cue to walk.
2. **Check rDNS control if you're running mail.** Receiving servers match your sending IP against its PTR record. Some hosts let you edit it yourself, some handle it by ticket, and some don't support it at all.

After deployment:

3. **Run the bind test.** Start a service on port 443 (or anything you care about) and connect from outside. On a dedicated IP this just works. On NAT it's impossible, because the port belongs to someone else's port map.
4. **Check the IP against blocklists** before you depend on it. A recycled IPv4 sometimes arrives pre-blacklisted thanks to a previous tenant. Most budget hosts will swap the address or let you destroy and redeploy cheaply, but it's better to find out on day one than after your first newsletter.

## RackNerd's KVM plans: the full dedicated-IP lineup and pricing

RackNerd's entire VPS lineup is standard KVM with a dedicated IPv4 included on every plan — there's no NAT tier to dodge and no per-IP add-on fee to factor in. The famous prices come from their annual-billing specials, which is where the value concentrates.

| Plan | RAM / vCPU | SSD | Traffic @ port | Annual price | Order |
| --- | --- | --- | --- | --- | --- |
| 512 MB KVM | 512 MB / 1 vCPU | 12 GB | 500 GB @ 1 Gbps | $8.99/yr | [Order the 512 MB plan](https://bit.ly/my-rn) |
| 1 GB KVM | 1 GB / 1 vCPU | 20 GB | 1 TB @ 1 Gbps | $11.29/yr | [Order the 1 GB plan](https://bit.ly/my-rn) |
| 2 GB KVM | 2 GB / 2 vCPU | 40 GB | 2 TB @ 1 Gbps | $18.99/yr | [Order the 2 GB plan](https://bit.ly/my-rn) |
| 2.5 GB KVM | 2.5 GB / 2 vCPU | 50 GB | 2.5 TB @ 1 Gbps | $24.99/yr | [Order the 2.5 GB plan](https://bit.ly/my-rn) |
| 3 GB KVM | 3 GB / 2 vCPU | 65 GB | 3 TB @ 1 Gbps | $29.99/yr | [Order the 3 GB plan](https://bit.ly/my-rn) |
| 4 GB KVM | 4 GB / 3 vCPU | 80 GB | 4 TB @ 1 Gbps | $38.99/yr | [Order the 4 GB plan](https://bit.ly/my-rn) |
| 5 GB KVM | 5 GB / 3 vCPU | 100 GB | 5 TB @ 1 Gbps | $44.99/yr | [Order the 5 GB plan](https://bit.ly/my-rn) |
| 6 GB KVM | 6 GB / 4 vCPU | 120 GB | 6 TB @ 1 Gbps | $54.99/yr | [Order the 6 GB plan](https://bit.ly/my-rn) |
| 7 GB KVM | 7 GB / 4 vCPU | 140 GB | 7 TB @ 1 Gbps | $64.99/yr | [Order the 7 GB plan](https://bit.ly/my-rn) |
| 8 GB KVM | 8 GB / 4 vCPU | 160 GB | 8 TB @ 1 Gbps | $74.99/yr | [Order the 8 GB plan](https://bit.ly/my-rn) |

Details that affect the decision:

- **Locations:** Los Angeles, San Jose, Seattle, Dallas, Chicago, New York, and Atlanta in the US, plus Amsterdam in Europe. You pick the datacenter at order time.
- **Billing shape:** the headline prices are annual. RackNerd also sells monthly and quarterly terms on its standard KVM line, but the specials are where the pricing gets silly — in a good way.
- **Provisioning** is automated and typically completes within minutes of payment. PayPal, major cards, and Alipay all work.
- **More IPs:** additional IPv4 addresses can be added on request for a small monthly fee, subject to justification.
- **Every plan includes full root access** with an OS template chosen at deploy, so a VPS bought for a VPN endpoint tonight can become a mail relay next month without reordering.

On tier selection: 512 MB handles a WireGuard node, a DNS box, or a tiny relay. The 1 GB plan is the single-service sweet spot. Once a web panel and a couple of daemons join, 2–3 GB earns its keep, and 4 GB upward is for busier applications or small production stacks.

## What a dedicated-IP VPS should cost: the annualized comparison

Billed annually, RackNerd is the cheapest option in this comparison by a wide margin — its entry plan costs less per year than most competitors charge per month. Teaser prices hide that, so here's everything converted to one unit:

| Provider | Cheapest listed plan | Cost over 12 months | Dedicated IPv4 | Worth knowing |
| --- | --- | --- | --- | --- |
| RackNerd | 1 GB / 20 GB SSD / 1 TB — $11.29/yr | **$11.29** | Included on every plan | Annual-billing specials; extra IPv4s on request |
| Cloudzy | 512 MB / 20 GB NVMe / 1 TB — $2.48/mo | $29.76 | Included, plus routable IPv6 /64 and self-service PTR | 13 regions; $2.48 is a 50%-off promo rate (list: $4.95/mo) |
| TruoCloud | 2 GB / 2 vCPU / 40 GB NVMe — $5.49/mo | ≈$49–66 depending on cycle | Included, with unmetered traffic | Daily backup and a 20-rule network firewall included |
| LightNode | 2 GB / 50 GB SSD / 1 TB — $7.71/mo | $92.52 | Included, across 40+ locations | Hourly pay-as-you-go billing available |

Two caveats keep this table honest. Entry specs differ — 512 MB against 1 GB against 2 GB — so you're comparing floors, not identical machines. And Cloudzy's figure holds only as long as its current 50% promotion does; at list price, the same plan runs $59.40 over a year. Even so, the gap comes from billing structure: annual billing concentrates the savings, and RackNerd's specials have stayed in the same range for years. Cloudzy balances its monthly pricing with a 14-day money-back window and editable PTR records, which is the fairer way to judge it.

If you're keeping a server twelve months or longer, the math isn't close. 👉 [See RackNerd's 1 GB KVM plan at $11.29/year](https://bit.ly/my-rn)

## When RackNerd is the wrong answer

Three situations call for a different shape of host, and pretending otherwise wastes your money.

**Short-lived or monthly-billed projects.** RackNerd's pricing assumes you keep the machine for a year; paying monthly there forfeits the discount that makes it interesting. For test boxes you'll destroy next week, a monthly host in the Cloudzy mold — or LightNode with its hourly billing — is the better tool, even at a higher monthly cost.

**Self-service rDNS and guaranteed IPv6.** Cloudzy ships editable PTR records and a routable /64 IPv6 block on every plan, no tickets involved. RackNerd supports custom rDNS, but it's handled through their panel or support rather than presented as a guaranteed self-service feature on every tier, and IPv6 availability varies by location. If mail deliverability is the entire reason for your purchase, confirm both in a pre-sales ticket before ordering. GhostVPS, for comparison, states openly that it doesn't support PTR editing at all — frustrating, but at least honest.

**Locations outside RackNerd's map.** Seven US cities plus Amsterdam covers most needs. If you specifically need Vietnam, the Middle East, or South America, hosts with 40+ locations are the practical route.

One more boundary worth stating: a dedicated IP stays with your server for its life — through reboots and reinstalls — but destroy the VPS and the address returns to the pool. If your architecture needs a floating IP that survives rebuilds, shop for that explicitly; Cloudzy sells floating IPs at $2.50/mo each.

## Ordering a RackNerd VPS: what happens after checkout

Checkout to SSH session usually takes minutes rather than hours. The flow looks like this:

1. Pick your plan and the annual cycle, then choose the datacenter at order time.
2. Pay — PayPal, major cards, and Alipay are all accepted.
3. Watch for the provisioning email with your IP and control panel login; automated setup typically finishes within minutes.
4. `ssh root@your.ip`, then run the first-hour checklist: confirm the IP matches the panel, run the blocklist check from earlier, set your hostname, and open the rDNS ticket if you're building a mail server.

👉 [Pick a datacenter and deploy a dedicated-IP KVM VPS](https://bit.ly/my-rn)

## FAQ

**Do all RackNerd VPS plans include a dedicated IP?**
Yes. Every KVM plan in the lineup includes at least one dedicated IPv4, and there are no NAT tiers in the standard catalog. Additional IPv4s can be requested for a fee when your use case justifies them.

**Can I run a mail server on a RackNerd VPS?**
Yes, with prep: set rDNS so the PTR matches your mail hostname, configure SPF/DKIM/DMARC, verify the IP isn't pre-blacklisted, and confirm outbound port 25 policy with support before you build
