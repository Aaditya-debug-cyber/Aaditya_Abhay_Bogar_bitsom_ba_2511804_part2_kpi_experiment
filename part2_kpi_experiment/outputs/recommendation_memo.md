# Recommendation Memo
**To:** Leadership  
**From:** Aaditya Abhay Bogar, Business Analyst (bitsom_ba_2511804)  
**Date:** 2026-06-19  
**Subject:** Campaign Experiment Decision — Onboarding & Activation Campaign  
**Recommendation:** Launch with Modifications

---

## 1. Executive Summary

The new onboarding and activation campaign produces a statistically significant improvement in paid conversion rate (+120.87% relative lift, p=0.0005), along with stronger user engagement and faster time-to-conversion. However, two guardrail metrics breach acceptable thresholds: the support ticket rate surged 67.7% in the Treatment group, and average revenue per converted user fell 52.7%. These signals indicate the campaign drives volume but introduces operational friction and revenue quality risk.

**Recommendation:** Proceed with a controlled, phased launch after addressing the identified onboarding friction points. Do not delay indefinitely — the conversion signal is strong and commercially significant — but do not launch without a support ticket mitigation plan.

---

## 2. Business Problem Statement

The company must decide whether to replace the existing onboarding experience with the new campaign-driven experience for all new users.

| Dimension | Detail |
|---|---|
| Decision to be made | Whether to roll out the new onboarding campaign to 100% of new users |
| Who is impacted | All new users, customer support teams, and the finance/revenue function |
| Metric that should improve | Paid conversion rate (primary) and revenue per user |
| Risks to monitor | Support ticket rate, refund rate, revenue quality per converted user |
| Evidence required | Statistical significance of conversion improvement + guardrail metrics within acceptable ranges |

---

## 3. North Star Metric

**Selected North Star: Paid Conversion Rate**  
Definition: (Users who converted to paid) / (Total users in group)

**Why this is the North Star:**
- It is the ultimate commercial outcome — every upstream metric (landing page visits, trial starts, onboarding completion) is only valuable insofar as it leads to a paid user
- It directly connects to Monthly Recurring Revenue (MRR) and long-term business growth
- It is unambiguous and measurable on a 30-day window

**Why not ARPU:** ARPU captures both the number of converters and the amount each pays. As a composite, changes in ARPU can be driven by either quantity or quality — making it harder to attribute campaign effects cleanly. Paid conversion rate isolates the quantity dimension.

**Risk of optimizing blindly:** A campaign could artificially inflate conversion by targeting lower-intent users who convert at low price points, depressing ARPC. This is exactly what we observe in this experiment — hence ARPC is a guardrail metric, not the North Star.

---

## 4. KPI Tree Explanation

The KPI tree maps Paid Conversion Rate to its underlying drivers:

**Driver 1 — Funnel Progression:** The proportion of users who move through each stage (landing page → trial → onboarding → paid) determines the overall conversion rate. The treatment improved every funnel stage, with the biggest gain at onboarding completion (+35% relative lift).

**Driver 2 — User Engagement Quality:** Engaged users are more likely to convert. The treatment group shows meaningfully higher engagement scores (+10.4%, p<0.0001) and converts 27.8% faster, suggesting the new experience creates stronger early product habits.

**Driver 3 — Revenue Quality:** Even as conversion volume improves, the average revenue per converted user must remain healthy. This driver is currently the most at-risk: ARPC fell 52.7%, driven primarily by Free-plan users converting at lower price points.

**Guardrail Metrics (minimum acceptable thresholds):**
- Support Ticket Rate ≤ 20% (Treatment: 24.8% — **BREACH**)
- ARPC decline ≤ 30% (Treatment: -52.7% — **BREACH**)
- Refund Rate < 1% (Treatment: 0.42% — within range, monitor)
- No segment showing net negative lift (all regions show positive lift — satisfied)

---

## 5. Experiment Result Summary

| Metric | Control | Treatment | Lift | Status |
|---|---|---|---|---|
| Users | 690 | 710 | +2.9% | Balanced |
| Landing Page Visit Rate | 63.6% | 72.4% | +13.8% | Positive |
| Trial Start Rate | 25.1% | 29.0% | +15.7% | Positive |
| Onboarding Completion Rate | 15.6% | 21.1% | +35.0% | Strong positive |
| **Paid Conversion Rate** | **3.19%** | **7.04%** | **+120.9%** | **Primary — significant** |
| ARPU | $52.0 | $54.3 | +4.4% | Mildly positive |
| ARPC | $1,630 | $770 | -52.7% | GUARDRAIL WARNING |
| Support Ticket Rate | 14.8% | 24.8% | +67.7% | GUARDRAIL WARNING |
| Refund Rate | 0.00% | 0.42% | n/a | Monitor |
| Avg Engagement Score | 57.0 | 62.9 | +10.4% | Positive |
| Avg Days to Convert | 8.9 | 6.4 | -27.8% | Positive |

---

## 6. Hypothesis Test Interpretation

**Test:** One-sided Z-test for two proportions (H1: p_treatment > p_control)  
**Significance level:** alpha = 0.05

| Test Output | Value |
|---|---|
| Z-statistic | 3.2640 |
| p-value | 0.0005 |
| 95% CI for difference | [+1.56 pp, +6.15 pp] |
| Decision | **Reject H0** |

The conversion improvement is statistically significant with high confidence. The probability of observing this result under the null hypothesis is 0.05% — effectively ruling out chance. The 95% confidence interval ([+1.56pp, +6.15pp]) is entirely positive, confirming the treatment effect is real.

A secondary t-test on engagement score (t=7.93, p<0.0001) further confirms the Treatment group had meaningfully better user engagement.

---

## 7. Guardrail Analysis

### Guardrail 1: Support Ticket Rate [BREACH]
- **Threshold:** ≤ 20% of users raising support tickets
- **Treatment result:** 24.8% (Control: 14.8%)
- **Risk:** The campaign generates nearly 1 in 4 users contacting support. At scale, this creates significant operational costs and indicates the onboarding experience has friction points or unmet expectations. If this rate holds post-launch, support team capacity will need to scale proportionally, eroding the campaign's unit economics.
- **Action required:** Identify the top support ticket categories in the Treatment group. Address the top 2–3 friction points before full launch.

### Guardrail 2: ARPC Decline [BREACH THRESHOLD]
- **Threshold:** Revenue per converted user should not fall more than 30%
- **Treatment result:** $770 vs $1,630 in Control (-52.7%)
- **Context:** Total ARPU is modestly higher in Treatment (+4.4%), because the campaign converts many more users. But each individual conversion is worth significantly less. This is driven by the Free plan segment — Treatment converts 9.3% of Free users (vs 3.1% in Control), likely into lower-tier paid plans.
- **Risk:** If LTV per user is proportional to ARPC, the long-term revenue impact may be weaker than the conversion numbers suggest.
- **Action required:** Run a 90-day LTV analysis post-launch to determine if Treatment converters upgrade over time, or remain at lower plan tiers.

### Guardrail 3: Refund Rate [WITHIN RANGE — WATCH]
- **Threshold:** < 1% of total users
- **Treatment result:** 0.42% (3 refunds vs 0 in Control)
- **Risk:** Low count but the appearance of refunds suggests some users are converting without full intent. Monitor closely in the first 30 days post-launch.

### Guardrail 4: Segment Parity [SATISFIED]
- All four regions show positive conversion lift in Treatment
- All device types show positive lift
- No segment is harmed by the campaign — this is a positive signal

---

## 8. Segment-Level Insights

### By Region
- All regions benefit: North shows the strongest lift (+155%), West the weakest (+50%)
- No region shows a negative effect — the campaign is broad in its impact

### By Device Type
- Mobile users drive the largest volume and show strong lift: 2.56% → 7.41% (+189%)
- Desktop shows moderate improvement: 4.5% → 6.6% (+46%)
- Tablet numbers are too small for reliable conclusions

### By Plan Type (Most Actionable Insight)
- **Free plan users:** 3.06% → 9.29% (+204% relative) — the campaign's primary beneficiaries
- **Basic plan users:** 3.62% → 3.88% (+7%) — minimal lift; campaign barely moves Basic users
- **Premium users:** 2.75% → 6.25% (+127%) — good lift but small absolute numbers

**Key insight:** The campaign is disproportionately effective at converting Free-plan users. This likely explains the ARPC drop — Free users convert at lower price points. Consider targeting the campaign specifically at Free users rather than all users, which would concentrate the conversion benefit and mitigate the ARPC dilution.

---

## 9. Final Recommendation

**Decision: Launch with Modifications (phased rollout)**

**Rationale:**
The conversion improvement is statistically robust (p=0.0005), commercially significant (+3.85 pp absolute), and consistent across all regions and device types. Engagement scores and time-to-convert both improve. These are strong positive signals.

However, the support ticket surge (24.8%) and ARPC decline (-52.7%) are material operational and financial risks. Launching without addressing these would create downstream problems.

**Recommended launch plan:**
1. **Immediate (within 2 weeks):** Audit the top support ticket categories in the Treatment group. Identify and fix the top 2–3 onboarding friction points.
2. **Phase 1 launch (30% traffic, Free plan users only):** Roll out to Free plan users — this segment drives the strongest lift and is the most commercially aligned with the campaign's conversion goal.
3. **Monitor for 30 days:** Track support ticket rate (target: bring below 20%), refund rate (<1%), and ARPC trajectory.
4. **Phase 2 launch (100% traffic):** If guardrail metrics are within range after 30 days, expand to all new users.

**Alternative considered: Do not launch**
Rejected. The conversion signal is too strong to abandon. The guardrail issues are addressable through operational improvements, not fundamental flaws in the campaign concept.

**Alternative considered: Launch only for Free plan users**
Valid partial alternative. If the timeline for full launch is too aggressive, a Free-only launch captures 65%+ of the campaign's conversion upside with a more contained support footprint.

---

## 10. Risks and Limitations

- **Support load risk:** If ticket rate does not improve post-launch, increased support costs could offset conversion gains
- **Revenue quality risk:** ARPC drop needs 90-day LTV monitoring to understand true commercial impact
- **Sample size limitations by segment:** Device-type and plan-type segments have small conversion counts (some <10), limiting the reliability of segment-level conclusions
- **30-day window:** Experiment ran only 30 days. Long-term retention and churn effects of the new onboarding experience are not yet observable
- **Novelty effect:** Some of the engagement and conversion uplift may reflect a short-term novelty effect that diminishes over time
- **Duplicate users:** 8 duplicate user IDs were removed. These could represent multi-session users or data pipeline issues — worth investigating

---

## 11. Next Steps

1. Audit support ticket categories in Treatment group (owner: Support team, timeline: 1 week)
2. Fix top 2–3 onboarding friction points identified in audit (owner: Product, timeline: 2 weeks)
3. Launch Phase 1 to Free plan users at 30% traffic (owner: Engineering, timeline: Week 3)
4. Monitor: Support ticket rate, refund rate, ARPC, conversion rate daily for 30 days
5. Run 90-day LTV analysis on Treatment converters (owner: Data team, timeline: Month 3)
6. Phase 2 decision based on 30-day monitoring results (timeline: Month 2)
