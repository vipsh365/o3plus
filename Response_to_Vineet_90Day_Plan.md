Dear Mr. Vineet Kapoor,

Thank you for the clarity. Having the mandate spelled out like this before we start makes a real difference. I've seen too many situations where things go sideways because people assumed alignment that wasn't actually there.

What follows is how I'd approach this, not in generalities, but in the specific mechanics of how each piece gets built, how they connect, and in what order. I haven't walked your floor yet, so some of this will get refined once I'm on the ground. But the architecture is sound. I've built this before.


## How I'd Sequence the Four Priorities

You've laid out four areas. I'd reorder them, and the reason comes down to dependencies.

**I'd start with Systems & Control, not Operations.**

Consolidation sounds like the most urgent thing, and in many ways it is. But if you consolidate operations before you have visibility into what's actually moving, what's sitting, and what's costing you money, you end up moving the mess from one place to another. You need the control system first. Then you consolidate with data, not gut feel.

I want to be direct about Navision. As installed today, it's effectively a dead end for where we need to go. It's a menu-driven, pre-SaaS system with no native dashboarding, minimal cloud capability, and older-generation APIs that don't integrate cleanly with e-commerce platforms, CRM tools, or modern third-party systems. Every integration needs middleware or heavy custom development, which is exactly the kind of patchwork that creates the manual-coordination problem we're trying to eliminate. Even where Navision is technically being used, the data doesn't surface in ways that support real-time decision-making, which is the whole point.

So this isn't a Navision refresh. It's a migration to a modern SaaS-based ERP. My strong lean is towards Oracle ERP Cloud as the primary candidate. It's fully SaaS, cloud-native, dashboard-first across every module (with the one exception of POS, which we'd handle separately), has a robust open API layer for our e-commerce and marketplace integrations, and is built for the kind of process manufacturing, multi-brand, multi-channel operation O3+ is. Dynamics 365 Business Central is a reasonable alternative to evaluate, but Oracle's maturity in process manufacturing and channel-wise finance is where I'd put my money. The final call gets made after the Day 1-30 audit, but the direction is clear: we move off Navision, we don't try to rescue it.

**Operations & Consolidation comes second.** Once I can see what's happening across plants and warehouses in real numbers, the consolidation towards Himachal becomes a project with clear cost benchmarks, not a logistics exercise based on estimates.

**Business Performance & Profitability is third,** because the hard questions (which of the 200+ SKUs are actually making money, which vendors overlap across O3+ Professional, DermaCult, Sara and the other labels, where formulations can be standardized) all need data that the ERP and MIS will generate. The 20-25% efficiency target is realistic for a business at this scale. But I've seen companies destroy revenue by rationalizing SKUs based on incomplete margin data. The sequencing matters.

**AI comes last in deployment, first in architecture.** We don't deploy AI tools on broken data. But from Day 1, every system decision ensures data is being captured cleanly and in structured formats, so that when we're ready to automate MIS or build a skin recommendation engine, we're not scrambling to clean up three years of messy records.


## The Foundation: 3-Year Budget and Organizational Architecture

Before I get into the 90-day breakdown, let me lay out the structural backbone that everything hangs on. This isn't a 90-day activity. It's a 2-3 year destination map. But the first 90 days is when we design it, validate it, and lock it in.

**The starting point is a 3-year business plan built top-down and bottom-up simultaneously.**

Top-down: we define the revenue target for Year 1, Year 2, Year 3, both top line and bottom line. Against that, we map the product range: which brands (O3+ Professional, DermaCult, Sara, Agelock, Biozoma, and any new labels), which categories within each brand, and the derived SKUs under each category. Every SKU gets a margin structure, not just gross margin, but channel-wise margin accounting for salon B2B pricing, modern trade margins, e-commerce commissions, general trade distributor margins, and export pricing. The point is to know, at a granular level, what each product actually earns in each channel after all costs.

Against that product-margin matrix, we define the sales verticals (salon/professional, modern trade, e-commerce including marketplace and D2C, general trade/wholesale, and export) and assign a revenue target to each vertical. Not a vague percentage, but a hard number: this vertical delivers this much in Year 1, grows to this much in Year 2, and so on. Each vertical has different unit economics, different sales cycles, and different pitch requirements, so the resource planning behind each one is different.

**This is where the organizational architecture comes in.**

Once the revenue targets and vertical assignments are clear, we derive the complete resource requirement. This is where most companies get it wrong. They hire based on what feels light, not based on what the plan actually demands.

Here's how I'd build it: from the 3-year budget, we cascade down to department-level requirements. For each department (sales, marketing, brand management, R&D, supply chain, production, quality, finance, HR) we define what that department needs to deliver to support the plan. Then within each department, we break it into sub-departments or functional roles. And for each role, we document four things clearly:

- **JD (Job Description):** what exactly this person does, not a generic HR template but a specific description tied to the business plan. The brand manager for DermaCult has a different JD than the brand manager for Sara, because the channels, price points, and consumer profiles are different.
- **KRA (Key Result Areas):** the 3-4 outcomes this role is accountable for, directly linked to the departmental targets which are in turn linked to the business plan.
- **KPI (Key Performance Indicators):** the measurable metrics we track weekly and monthly to know if the KRAs are being met. Not vague indicators, but specific numbers: revenue per territory for a sales person, formulation-to-approval cycle time for R&D, fill rate for supply chain.
- **Capability and background requirement:** what skills, experience level, and salary band this role demands.

This cascade (from budget to department to sub-department to individual role to JD/KRA/KPI) gives us two things simultaneously. First, it tells us exactly how many people we need and where. Second, it tells us who in the current team fits, who needs training, and where we have gaps that require hiring.

**The evaluation and review system is what makes this sustainable, not just a one-time exercise.**

I'd implement this on Darwinbox as the HRMS backbone. It handles the org structure, JD/KRA/KPI documentation, attendance, leave, payroll, and the review cycle in one system. But on top of Darwinbox, I'd layer a modulated AI tool that does something most HRMS platforms can't do well: continuous performance scanning.

Here's what I mean practically. From the business plan, we create a task sheet for each department. The task sheet says: this is the revenue you need to support, these are your deliverables broken down monthly and weekly. Every individual within that department gets a derived task sheet, their weekly deliverables mapped from the department targets. The AI tool scans completion, quality, and timeliness of these tasks. It's not just tracking whether someone logged in or attended meetings. It's checking actual output against expected output.

Based on that continuous scan, we identify three categories of people:

1. People performing well: they get recognized, given more responsibility, and become candidates for the leadership pipeline.
2. People who are underperforming but capable: they need targeted training, a workshop, or sometimes just clearer direction. We invest in them. Run the training, check again in 2-3 weeks.
3. People who consistently underperform even after intervention: they get flagged. After a clear review cycle with documented feedback, they're either moved to a different role where their skills fit better, or they're transitioned out.

This isn't about being harsh. It's about being honest. And it protects the good people, because nothing kills a strong team faster than carrying people who aren't pulling their weight while everyone else compensates.

The output of this entire system is a clear, data-backed picture at any given time: who we have, what they're delivering, where training is needed, what positions need to be hired for, and how the organizational capability maps to the business plan. No guessing.


## Product Strategy: Master Products, Families, and the Push-Pull Engine

This is where the revenue actually gets built, and it needs to be done with discipline, not just creative instinct.

**For each sales segment, we need to identify or create a master product.**

The master product is the anchor. It's the SKU that defines the brand's presence in that segment. It has to be differentiated enough to stand out, but highly consumable so it generates repeat purchase. It's what the sales team leads with, what the marketing spends are concentrated on, and what the consumer associates with the brand in that category. Think of it as the rocket: it goes up first, it's visible, and everything else rides behind it.

Around each master product, we build a family of smaller, complementary SKUs. These smaller products serve two purposes: they get pulled by the master product (a consumer buying the hero facial kit is naturally led to the serum, the sunscreen, the maintenance cream in the same range), and they also push the master product from below (someone who starts with a smaller, lower-commitment product and has a good experience gets pulled up to the full range). This push-pull dynamic is how you build a product family that generates ROI on both horizontal and vertical scale. Horizontal meaning more consumers across more touchpoints, vertical meaning higher per-consumer spend through the product ladder.

**But this strategy only works if the product development process behind it is rigorous.**

It starts with brand management. Before any product goes to R&D, the brand manager has to deliver a complete brief: market survey findings, competition analysis (both Indian and international benchmarks), target consumer profile, the specific USPs we want to inculcate in the formulation, the ingredient story, the pricing architecture and margin structure at each channel level, and a clear articulation of why this product should exist. Not "the market needs another serum" but "based on competition mapping and consumer research, there's an underserved need for X, and if we formulate with these specific actives, we can claim these USPs and price it here, with this margin at each level."

R&D receives that brief and develops multiple formulation options. These go through in-house testing first, then consumer-facing evaluation, whether that's panel testing, salon trials, or controlled sampling. The formulations get shortlisted. Pricing is validated against the consumer response. Packaging is derived, and here's an important efficiency point: packaging should be designed for multiple-use potential wherever possible. If a bottle format, a tube size, or a jar spec can serve across two or three products or brands, that's procurement leverage and production simplification without compromising how the product looks on shelf.

Once the product is locked (formulation, packaging, pricing, positioning), the 360-degree marketing framework kicks in. The marketing team prepares everything the sales channels need: digital marketing plan (platform-specific, not generic), offline event strategy, the specific pitch and talking points for sales teams, sample kits, marketing collateral, and the channel-specific projection of how much volume goes where. Modern trade has a different shelf strategy than e-commerce listings than salon introduction kits than export documentation. Each gets planned separately.

The sales team then goes to each vertical with a clear brief: this is the product, this is how you pitch it, this is your target, and this is how success gets measured. Every sales vertical (salon B2B, modern trade, e-commerce, wholesale, export) has a different discourse, a different buyer psychology, and a different projection cycle. What works as a salon introduction won't work as an Amazon listing strategy. This gets planned, not improvised.


## Supply Chain Integration and Live MIS Control

Everything I've described above (budget planning, resource deployment, product development, multi-vertical sales) collapses if the supply chain can't deliver.

**The supply chain needs to be designed as a single integrated flow, not a collection of departments handing things off.**

It starts with forecasting. Based on the sales vertical targets and the product launch calendar, we derive a manufacturing forecast: how much of which SKU needs to be produced in which month. That forecast drives procurement: raw material requirements and packaging material requirements, planned in advance with clear lead times, not ordered reactively when stock runs out.

This is where the departmental integration map becomes critical. Every department has inputs it receives and outputs it delivers, and those outputs become inputs for the next department. Brand management's brief is R&D's input. R&D's approved formulation is production's input. Production's schedule is procurement's input. Procurement's delivery timeline is warehousing's input. Warehousing's dispatch schedule is sales' input. Sales' feedback is CRM's input, which circles back to brand management for the next cycle.

I map this entire chain visually, on what I call an A2 layout where every department sits on the map with its input-output connections clearly drawn. Where any handoff is currently informal, person-dependent, or prone to delays, we design the system-enforced alternative. No department operates in isolation. When we systematize one function, we make sure we're not breaking the handoff to the next.

**Live MIS dashboards run across all of this.**

Not monthly reports. Not weekly summaries someone compiles in Excel. Real-time dashboards that show: current inventory position (raw material, packaging, WIP, finished goods), production status against schedule, dispatch status against orders, procurement pipeline with expected delivery dates, and sales performance against targets by vertical. Leadership (you, Vidur, and the department heads) should be able to open a screen at any point and know exactly where things stand. When something slips, the dashboard flags it before someone has to discover it manually.

**FIFO and inventory aging management needs to be systematic, not reactive.**

Every item in inventory, whether it's a raw material ingredient, a packaging component, or a finished product, gets tracked with its entry date, batch number, and shelf life. FIFO (first in, first out) is enforced at the warehouse level through system controls and physical layout design, not left to the judgment of whoever's picking stock that day.

For aging inventory, we maintain a live aging sheet that categorizes everything into time bands (0-6 months, 6-12 months, 12-24 months, beyond 24 months). Each band triggers a different action protocol:

- Fresh stock (0-6 months): normal sales flow.
- Aging stock (6-12 months): priority allocation to faster-moving channels, bundled into schemes or combo offers.
- At-risk stock (12-24 months): active liquidation through cross-selling into adjacent channels, barter arrangements with partners, discounted clearance through specific outlets (without damaging brand perception in primary channels), or reprocessing where ingredient stability allows.
- Dead stock (beyond 24 months or approaching expiry): evaluated for any salvage value, written off where necessary, and critically, root-cause analyzed so the procurement or forecasting error that created it gets fixed in the system to prevent recurrence.

The goal isn't just to clear old stock. It's to build a system where old stock stops accumulating in the first place, because procurement is forecast-linked, production is demand-driven, and the MIS flags slow-moving items early enough to act before they become a problem.


## The Lean Manufacturing Toolkit

The ERP and MIS give us visibility. Lean manufacturing discipline is what actually moves the efficiency needle on the plant floor. For a multi-SKU cosmetics operation like Parwanoo, the specific tools I'd put in place:

- **SMED (Single-Minute Exchange of Die):** With 200+ SKUs sharing production lines, changeover time between batches is one of the biggest hidden losses. SMED restructures changeover into internal and external steps so lines come back up faster. Typical target: cut changeover time by 40-50% within the first year.
- **Value Stream Mapping:** Before optimizing any line, we map the full value stream for each product family to see where waste actually lives (waiting, over-production, excess inventory, rework, motion). This is what tells us which lean tools to apply where, rather than defaulting to generic programs.
- **5S and Visual Factory:** Sort, Set in order, Shine, Standardize, Sustain. Sounds basic. It isn't. A disciplined 5S implementation on the shop floor, combined with visual andon boards showing line status, batch progress, and deviations in real time, is what makes the MIS dashboards meaningful at the operator level too.
- **TPM (Total Productive Maintenance):** Equipment downtime kills yield. TPM moves maintenance from reactive to planned and brings operators into first-line equipment care. Pairs with OEE (Overall Equipment Effectiveness) as the tracking metric.
- **Kanban and pull-based scheduling:** Once forecasting is reliable, production shifts from push (make to stock) to pull (make to demand signal). Reduces WIP, reduces finished goods inventory, improves freshness at dispatch. Especially valuable for the high-velocity SKUs.
- **Poka-yoke (error proofing):** Particularly important in formulation dosing and labeling. Physical and system-enforced checks that make it impossible to use the wrong ingredient, the wrong packaging, or the wrong label on a batch.
- **Kaizen cycles:** Weekly cross-functional improvement cycles owned by operators and supervisors, not consultants. Small, compounding improvements beat big-bang projects.
- **MES layer (Manufacturing Execution System):** Sitting between the ERP and the shop floor. Captures real-time batch data, operator actions, equipment status, and quality checks. Feeds the MIS with live production data rather than end-of-shift manual entries.

These aren't theoretical frameworks. Each one has a direct line to cost, quality, or throughput, and each one feeds structured data back into the MIS so we can measure whether the discipline is sticking. Rolled out in sequence (5S and VSM first, then SMED and TPM, then Kanban and MES integration), this is where a meaningful chunk of the 20-25% efficiency target gets delivered.


## Where the 20-25% Efficiency Comes From

The target isn't a single lever. It's the compounding effect of formulation standardization, packaging rationalization, vendor consolidation, forecast-driven production, tighter inventory carrying costs, and channel-specific margin optimization. None of these are revolutionary on their own. The leverage is in running them as a connected system rather than piecemeal.


## First 90 Days: Execution Plan

With the architecture above as the destination, here's how the first 90 days unfold.

---

**Days 1-30: Diagnose and Design**

I'd spend most of this time in Himachal. Walk the plant, sit in the warehouse, understand the procurement cycle, watch how production scheduling actually happens versus how it's supposed to happen. Separately, sit with every department that touches Navision and figure out what's being used, what's being worked around, and what's missing.

Full inventory assessment: raw materials, packaging, finished goods across every location. Aging, shelf life, movement velocity, accumulation patterns. This tells me the real state of operations faster than any report can.

In parallel, I'd work with finance and the commercial team to start building the 3-year budget framework, pulling historical revenue data by product, by channel, by vertical, and mapping it against cost structures to establish the baseline. You can't plan where you're going without being honest about where you are.

I'd also spend real time with Vidur and the department heads. Working sessions, not formal meetings. I need to understand who runs what, where the institutional knowledge lives, and who has the appetite to drive change. The 3-4 people who form the core transformation team will reveal themselves here.

**What I'd start building immediately:**

A daily MIS report. Manual if necessary, but structured and non-negotiable. Every morning, leadership sees yesterday's production, dispatch, and inventory position. The format mirrors what the automated system will produce. People start getting used to reading numbers daily instead of flying blind between monthly reviews.

A basic BI dashboard (Power BI) pulling from whatever data exists today (Navision, spreadsheets, whatever). It won't be pretty, but it needs to show stock position, aging, and movement so we can make faster decisions while the bigger system overhaul is underway.

**What I'd stop:**

If procurement is happening without demand linkage, that stops immediately. Similarly, new SKU launches without a full business case (margin analysis, channel fit, cannibalization check) get paused. R&D pipeline stays active. Nothing enters production without going through the gate.

---

**Days 31-60: Build the Framework**

This is when the architecture described earlier gets committed to paper and to systems. The 3-year budget is locked (revenue, brand mix, channel targets, margin architecture). The organizational resource plan is built from that budget, with JD/KRA/KPI documented for every role. Darwinbox goes live as the HRMS backbone, with the AI task-scanning layer scheduled for Month 3-4.

On the product and supply side, the SKU profitability analysis completes, master products per segment are either identified or flagged for development, and the vendor and formulation overlap map is built. The warehouse consolidation blueprint is finalized with FIFO zones and batch traceability designed in. The A2 department integration map is documented.

Execution side: ERP modernization kicks off. Based on the audit, we finalize the path (Oracle ERP Cloud as the leading candidate, with Dynamics 365 Business Central as the alternate), define Phase 1 scope (inventory, production planning, procurement, dispatch, finance), bring in an implementation partner with Indian FMCG experience, and lock a go-live target.

---

**Days 61-90: Execute and Measure**

Physical consolidation Phase 1 begins, likely finished goods warehousing into the new layout. This sets the template before we tackle raw material and production moves.

First hires against the resource plan are initiated. Training programs for identified skill gaps begin. The AI performance tracker goes into pilot in 1-2 departments, scanning task completion against the weekly task sheets. The skin recommendation engine is scoped with a technical partner and a prototype timeline set.

**Day 90 is a proper checkpoint.** I sit down with you and Vidur and go through everything: what we found, what's done, what's in progress, what the next 90 days focus on. If something turned out harder than expected, better to say that clearly and adjust.


## How I Work

I'll be in Himachal most of the first few months. These things don't get fixed over video calls. I need to see how the plant runs day to day, talk to the guys on the floor, understand what's really happening versus what shows up in reports.

On Vidur: I'd want him involved in the actual decisions, not just briefed after the fact. Whatever we build over the next two to three years, he needs to be able to run it, question it, and change it when the business demands something different. That only works if he's been in the room when the tradeoffs were made.

I respect what O3+ has achieved. 21 years, bootstrapped, 50,000+ salons, nearly Rs 400 crore. You don't get there without real substance. The job now is to put the systems and processes underneath all of that so the next phase of growth doesn't depend on any one person holding everything together.

Happy to discuss any of this in more detail.

Best,
Vipul
