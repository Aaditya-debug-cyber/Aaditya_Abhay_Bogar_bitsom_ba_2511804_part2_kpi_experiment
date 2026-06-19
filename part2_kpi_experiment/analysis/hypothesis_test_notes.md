# Hypothesis Test Notes
**Analyst:** Aaditya Abhay Bogar (bitsom_ba_2511804)  
**Date:** 2026-06-19  
**Experiment:** Onboarding & Activation Campaign — Control vs Treatment

---

## 1. Business Context for Hypothesis

The company launched a new onboarding campaign to improve user conversion. Leadership needs to know whether the treatment should be rolled out to all users. The hypothesis test provides the statistical evidence required to determine whether observed differences are real or could have arisen by chance.

---

## 2. Primary Metric Tested

**Metric:** Paid Conversion Rate (`converted_to_paid`)  
**Definition:** Proportion of users in each group who converted to a paid subscription within 30 days

**Why this metric:**
- Paid conversion is the North Star metric — it is the most direct measure of business value
- It is binary (0/1), making it well-suited for a proportion Z-test
- All other funnel metrics (landing page visit, trial start, onboarding completion) are intermediate steps; only paid conversion represents realized revenue
- This metric connects directly to the business decision: does the campaign drive meaningful paid growth?

---

## 3. Hypotheses

**H0 (Null Hypothesis):**  
The paid conversion rate in the Treatment group is less than or equal to the conversion rate in the Control group.  
`H0: p_treatment <= p_control`

**H1 (Alternate Hypothesis):**  
The paid conversion rate in the Treatment group is greater than the paid conversion rate in the Control group.  
`H1: p_treatment > p_control`

---

## 4. Test Design

| Parameter | Value |
|---|---|
| Test type | One-sided Z-test for two independent proportions |
| Why one-sided | The business question is directional: "Is Treatment better?" Not "Is Treatment different?" |
| Significance level (alpha) | 0.05 (5%) |
| Decision rule | Reject H0 if p-value < 0.05 OR Z-statistic > 1.6449 (critical value) |
| Alternative | `p_treatment > p_control` (right-tailed) |

---

## 5. Test Inputs

| Input | Value |
|---|---|
| Control group size (n_c) | 690 |
| Treatment group size (n_t) | 710 |
| Control converters (k_c) | 22 |
| Treatment converters (k_t) | 50 |
| Control conversion rate (p_c) | 0.0319 (3.19%) |
| Treatment conversion rate (p_t) | 0.0704 (7.04%) |
| Absolute difference (p_t - p_c) | +3.854 percentage points |
| Relative lift | +120.87% |

---

## 6. Test Calculation

```
Pooled proportion:
  p_pool = (k_t + k_c) / (n_t + n_c)
         = (50 + 22) / (710 + 690)
         = 72 / 1400
         = 0.05143

Standard Error (pooled):
  SE = sqrt(p_pool × (1 - p_pool) × (1/n_t + 1/n_c))
     = sqrt(0.05143 × 0.94857 × (1/710 + 1/690))
     = sqrt(0.05143 × 0.94857 × 0.002854)
     = 0.011807

Z-statistic:
  Z = (p_t - p_c) / SE
    = (0.0704 - 0.0319) / 0.011807
    = 0.03854 / 0.011807
    = 3.2640

p-value (one-sided, right tail):
  p = P(Z > 3.264) = 0.0005

95% Confidence Interval for (p_t - p_c):
  SE_diff = sqrt(p_t(1-p_t)/n_t + p_c(1-p_c)/n_c)
           = sqrt(0.0704×0.9296/710 + 0.0319×0.9681/690)
           = 0.01162
  CI = [0.03854 - 1.96×0.01162, 0.03854 + 1.96×0.01162]
     = [0.0156, 0.0615]
     = [+1.56 pp, +6.15 pp]
```

---

## 7. Test Output

| Result | Value |
|---|---|
| Z-statistic | **3.2640** |
| Critical value (alpha=0.05, one-sided) | 1.6449 |
| p-value (one-sided) | **0.0005** |
| 95% CI for difference | **[+1.56 pp, +6.15 pp]** |
| Decision | **REJECT H0** |

---

## 8. Interpretation

**Statistical conclusion:** The result is statistically significant at the 5% level (and even at the 0.1% level). The Z-statistic of 3.264 far exceeds the critical value of 1.645. The p-value of 0.0005 means there is only a 0.05% chance of observing a difference this large by random chance if H0 were true.

**Confidence interval:** The 95% CI for the true difference is [+1.56 pp, +6.15 pp]. Since the entire interval is positive, we are 95% confident that the treatment genuinely improves conversion — not just in this sample, but in the population.

**Business interpretation:**
- The new campaign produces a genuine, measurable improvement in paid conversion
- Treatment converts 7.04% of users vs 3.19% in control — a 120.87% relative improvement
- The effect size is practically meaningful: doubling conversion rate represents significant revenue impact
- **However**, statistical significance alone is not sufficient for a launch decision. See guardrail analysis below.

---

## 9. Secondary Test: Engagement Score

**Metric:** Average Engagement Score  
**Test:** One-sided Welch's t-test (unequal variances assumed)  
**H0:** Mean engagement in Treatment <= Mean engagement in Control  
**H1:** Mean engagement in Treatment > Mean engagement in Control

| Result | Value |
|---|---|
| Control mean | 57.03 (n=684) |
| Treatment mean | 62.94 (n=702) |
| Difference | +5.91 points |
| t-statistic | 7.9282 |
| p-value | < 0.0001 |
| Decision | **REJECT H0** |

Engagement score is also significantly higher in Treatment — reinforcing that the campaign improves user experience and engagement quality.

---

## 10. Guardrail Considerations (not part of the formal test, but required for decision)

Despite the statistically significant conversion improvement, three guardrail signals require attention before a full launch:

| Guardrail | Control | Treatment | Status |
|---|---|---|---|
| Support Ticket Rate | 14.78% | 24.79% | **WARNING: +67.7% — exceeds 20% threshold** |
| ARPC (rev per converter) | $1,630 | $770 | **WARNING: -52.7% decline in revenue quality** |
| Refund Rate | 0.00% | 0.42% | Watch: 3 refunds appeared (small but monitor) |

The support ticket surge is the most serious concern. It suggests the new onboarding experience may be creating confusion or unmet expectations, leading users to seek help. If not addressed, increased support load will erode the unit economics of the campaign.

---

## 11. Interpretation Logic Summary

| Evidence | Direction | Weight |
|---|---|---|
| Paid conversion (Z-test) | Treatment > Control (p=0.0005) | Very strong positive |
| Engagement score (t-test) | Treatment > Control (p<0.0001) | Strong positive |
| Funnel progression (all stages) | Treatment > Control | Positive |
| Days to convert | Treatment faster (-27.8%) | Positive |
| Support ticket rate | Treatment much higher (+67.7%) | Negative (guardrail breach) |
| ARPC | Treatment much lower (-52.7%) | Negative (guardrail concern) |
| Refund rate | Treatment higher (0% → 0.42%) | Negative (minor, monitor) |

**Overall verdict:** The campaign works on its primary metric but ships with meaningful operational risk. The recommended action is a **conditional launch** — proceed with fixes to the onboarding friction points driving support tickets, and monitor ARPC closely after launch.
