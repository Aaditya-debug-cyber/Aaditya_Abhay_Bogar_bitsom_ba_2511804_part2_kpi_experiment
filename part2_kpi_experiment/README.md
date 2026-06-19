# Part 2: KPI Framework, Business Experiment Analysis & Decision Recommendation

**Student:** Aaditya Abhay Bogar  
**Student ID:** bitsom_ba_2511804  
**Repository:** `aadityaabhaybogar_bitsom_ba_2511804_part2_kpi_experiment`

---

## Business Context

A subscription-based digital product company launched a new onboarding and activation campaign. Users were split into a Control group (existing onboarding) and a Treatment group (new campaign experience). Leadership must decide whether to roll out the campaign to all users. This analysis frames the business problem, defines the right success metrics, analyzes experiment results, evaluates guardrail metrics, and produces a data-driven recommendation.

---

## Dataset Description

| Attribute | Value |
|---|---|
| File | `data/campaign_experiment_data.xlsx` |
| Raw records | 1,408 |
| Records after dedup | 1,400 |
| Control users | 690 |
| Treatment users | 710 |
| Columns | 16 |
| Key columns | user_id, experiment_group, region, device_type, traffic_source, plan_type, visited_landing_page, started_trial, completed_onboarding, converted_to_paid, revenue_30d, support_tickets_30d, refund_requested, days_to_convert, engagement_score |

---

## North Star Metric Selected

**Paid Conversion Rate** — defined as the proportion of users who converted to a paid subscription within 30 days.

**Rationale:**
- Most direct measure of business value (revenue realization)
- Clearly attributable to the campaign's stated objective
- Unambiguous, binary, measurable on a 30-day window
- All other funnel metrics are intermediate steps toward this outcome

**What could go wrong if optimized blindly:** The campaign could inflate conversion volume by targeting lower-intent users who pay less, causing ARPC to fall. This is observed in this experiment — ARPC declined 52.7% in the Treatment group.

---

## KPI Tree Summary

The KPI tree (`outputs/kpi_tree.png`) maps Paid Conversion Rate to three primary drivers:

**Driver 1 — Funnel Progression:** Landing Page Visit Rate → Trial Start Rate → Onboarding Completion Rate → Paid Conversion. Treatment improved all four funnel stages.

**Driver 2 — User Engagement Quality:** Engagement Score, Days to Convert, Feature Adoption Rate. Treatment users are more engaged and convert faster.

**Driver 3 — Revenue Quality:** ARPU, ARPC, Premium Plan Upgrade Rate. Treatment ARPC is concerning (-52.7%).

**Guardrail Metrics:**
1. Support Ticket Rate (threshold ≤ 20%) — Treatment: 24.8% [BREACH]
2. ARPC Decline (threshold < -30%) — Treatment: -52.7% [BREACH]
3. Refund Rate (threshold < 1%) — Treatment: 0.42% [Within range, monitor]
4. Segment-level parity — All regions/devices show positive lift [Satisfied]

---

## Experiment Analysis Approach

**Data quality checks performed:**
- Removed 8 duplicate user_ids (kept first occurrence)
- Identified and documented missing values: device_type (18), traffic_source (24), engagement_score (14)
- days_to_convert is null for non-converters — expected, not a data error
- Validated all binary columns: 0/1 values only, no invalid entries
- Flagged 15 revenue outliers above the 99th percentile (retained)
- Verified near-balanced group split (690 vs 710)

**Segment analyses performed:**
- By Region (East, North, South, West)
- By Device Type (Mobile, Desktop, Tablet)
- By Plan Type (Free, Basic, Premium)
- Full funnel breakdown (5-stage funnel)

---

## Hypothesis Test Summary

**Test:** One-sided Z-test for two independent proportions  
**H0:** p_treatment <= p_control (no improvement)  
**H1:** p_treatment > p_control (campaign improves conversion)  
**Significance level:** alpha = 0.05  

| Result | Value |
|---|---|
| Z-statistic | 3.2640 |
| p-value | 0.0005 |
| 95% CI for difference | [+1.56 pp, +6.15 pp] |
| Decision | **Reject H0 — statistically significant** |

A secondary Welch's t-test on engagement score (t=7.93, p<0.0001) also confirms significant improvement.

---

## Guardrail Metrics Considered

| Guardrail | Threshold | Control | Treatment | Status |
|---|---|---|---|---|
| Support Ticket Rate | ≤ 20% | 14.8% | 24.8% | WARNING — BREACH |
| ARPC Decline | < -30% | $1,630 | $770 | WARNING — BREACH (-52.7%) |
| Refund Rate | < 1% | 0.00% | 0.42% | Monitor |
| Segment Parity | All segments positive | — | All positive | Satisfied |

---

## Final Recommendation

**Decision: Launch with Modifications (phased rollout)**

The conversion improvement is statistically robust and commercially significant. However, the support ticket rate (24.8%) and ARPC decline (-52.7%) present real operational and revenue risks that must be addressed before full launch.

**Recommended plan:**
1. Audit and fix top support friction points in the new onboarding flow (2 weeks)
2. Phase 1: Launch to Free plan users at 30% traffic (Free users show the strongest lift: +204%)
3. Monitor guardrail metrics for 30 days
4. Phase 2: Full launch if support ticket rate falls below 20% and ARPC stabilizes

---

## Assumptions and Limitations

- Duplicate user_ids assumed to be data entry errors; first record kept
- days_to_convert null for non-converters — treated as expected, not missing
- Revenue outliers retained (may be legitimate high-value users)
- 30-day window only — long-term retention and churn not observable
- Small cell counts in some segments (Tablet, Premium) limit reliability of segment conclusions
- Novelty effect cannot be ruled out — engagement gains may diminish over time

---

## Screenshots Included

| File | Contents |
|---|---|
| `screenshots/summary_metrics.png` | Control vs Treatment comparison: funnel rates, revenue, guardrails, summary table |
| `screenshots/hypothesis_test_output.png` | Full Z-test calculation, inputs, result, and business interpretation |
| `screenshots/kpi_tree_preview.png` | KPI tree with North Star, drivers, sub-drivers, and guardrail metrics |

---

## Repository Structure

```
part2_kpi_experiment/
├── data/
│   └── campaign_experiment_data.xlsx
├── analysis/
│   ├── experiment_analysis.xlsx     # Data quality checks + group distribution
│   └── hypothesis_test_notes.md     # Full hypothesis setup, calculation, interpretation
├── outputs/
│   ├── kpi_tree.png                 # KPI tree visual
│   ├── experiment_summary.xlsx      # Control vs Treatment comparison (5 sheets)
│   └── recommendation_memo.md       # Full recommendation with business reasoning
├── screenshots/
│   ├── summary_metrics.png
│   ├── hypothesis_test_output.png
│   └── kpi_tree_preview.png
└── README.md
```
