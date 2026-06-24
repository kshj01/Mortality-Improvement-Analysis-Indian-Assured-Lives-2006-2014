# Mortality-Improvement-Analysis-Indian-Assured-Lives-2006-2014

An Excel-based Actuarial pricing study examining the impact of mortality improvement on Term Assurance premiums using published Indian mortality tables (IALM 2006-08 and IALM 2012-14).

## Objective

To quantify how mortality improvement between two investigation periods affects gross Term Assurance premiums, decompose source of premium change and Stress test pricing assumptions.

## Methodology
- Life tables construction (qx, px, lx, dx, force of mortality) for both IALM vintages.
- Gross Premium calculations using Equivalence principle under UDD (mid-year death benefit timing).
- Premium decomposition : total change attributed to EPV Benefit, EPV Renewal loading, denominator effect and interaction term.
- Mortality stress testing ($$\pm$$ 25%, $$\pm$$ 20%, $$\pm$$ 15%, $$\pm$$ 10%) with dynamic age selection.
- Interest rate sensitivity analysis ($$\pm$$ 1%, $$\pm$$ 0.5%).

## Key Finding
- Mortality improvement reduced gross premiums by 6.1%–9.3% across entry ages 25–45.
- The reduction is largest at older entry ages (age 45: −9.1%), reflecting greater absolute mortality improvement at higher ages.
- 98.9% of premium reduction is attributable to lower EPV of death benefit; expense and denominator effects are negligible.
- A ±10% mortality stress moves premiums by approximately $$\pm$$ 9%; a $$\pm$$ 1% interest rate shift moves premiums by less than ±0.2%, confirming mortality assumption risk dominates interest rate risk for short-duration term assurance.

## Data Sources
- IALM 2006-08: Institute of Actuaries of India (effective 1 April 2013)
- IALM 2012-14: Institute of Actuaries of India (effective 1 April 2019)

## Assumptions
- Sum Assured: ₹50,00,000
- Policy term: 10 years, annual premiums payable in advance
- Interest rate: 6.5% per annum
- Initial expenses: ₹2,000 | Renewal expenses: ₹50 per premium
- Initial commission: ₹1,500 | Renewal commission: ₹40 per premium
- Death benefit paid immediately on death (UDD assumption)
- Age convention: age last birthday (IALM 2012-14); age nearest birthday (IALM 2006-08) — treated as comparable for round entry age.

## Workbook Structure

### Sheet 1 — IALM Comparison
Raw mortality data and life table construction for both IALM vintages 
side by side.

**Contents:**
- qx for IALM 2006-08 (ages 2–115, age nearest birthday) and IALM 
  2012-14 (ages 2–115, age last birthday)
- Full life table functions per table: lx (radix 10,000), dx, px, 
  force of mortality μx (= −ln(px), constant force within year assumption)
- Mortality improvement column: proportionate reduction in qx between 
  tables, with status flag (Improved / Deteriorated)
- Pivot summary: average improvement by age band (0–9, 10–19, 20–29, 
  30–39, 40–49, 50+), flagging that ages 0–9 show disproportionately 
  large improvements and may distort overall averages

**Note:** Age convention differs between tables (nearest vs last birthday);
treated as comparable for round entry ages with caveat documented.

---

### Sheet 2 — Calculations
Central assumptions hub and EPV workings for all five policyholders 
under both mortality tables.

**Contents:**
- Named input cells: Sum Assured, interest rate (i), discount factor (v), 
  initial/renewal expenses, initial/renewal commission
- Per policyholder (A–E, ages 25–45): full year-by-year calculation of 
  tpx, discount factors vt, EPV of death benefit (mid-year UDD timing), 
  EPV of expenses, EPV of commission — computed separately under IALM 
  2006-08 and IALM 2012-14
- All pricing sheet outputs reference named ranges defined here, so a 
  single change to any assumption propagates automatically across the 
  entire workbook

---

### Sheet 3 — Pricing Term Assurance
Gross premium derivation for all five policyholders under both tables.

**Contents:**
- Annuity-due factor (äx:10|) and term assurance factor (A¹x:10|) 
  per policyholder per table
- EPV of benefit, EPV of expenses, EPV of commission displayed 
  separately before aggregation
- Gross premium via Equivalence Principle:
  P = (EPV Benefit + EPV Initial Expenses + EPV Initial Commission 
      + EPV Renewal Expenses + EPV Renewal Commission) / äx:10|
- Death benefit treated as immediate payment under UDD: 
  A¹x:10|(immediate) computed recursively using v^0.5 mid-year 
  discount within each policy year

---

### Sheet 4 — Premium Comparison
Side-by-side premium comparison and source-of-change decomposition.

**Contents:**
- Premium table: IALM 2006-08 vs 2012-14 premiums for all five 
  policyholders with absolute (ΔP) and percentage change columns
- Decomposition of ΔP into four attributed components, isolating 
  the contribution of each separately:
  - ΔP Benefit: SA × (A¹₁₂₁₄ − A¹₀₆₀₈) / ä₀₆₀₈
  - ΔP Renewal: (e+c) × (ä₁₂₁₄ − ä₀₆₀₈) / ä₀₆₀₈  
  - ΔP Denominator: N₀₆₀₈ × (1/ä₁₂₁₄ − 1/ä₀₆₀₈)
  - Interaction term: (N₁₂₁₄ − N₀₆₀₈) × (1/ä₁₂₁₄ − 1/ä₀₆₀₈)
- % of Total summary row: each component's share of aggregate ΔP 
  across all five policyholders
- Conclusion paragraph: narrative interpretation of decomposition 
  results

**Key result:** 98.9% of premium reduction attributed to lower EPV of 
death benefit; renewal loading and denominator effects together account 
for under 1.1%.

---

### Sheet 5 — Stress Test on IALM 2012-14
Dynamic stress testing module with interactive controls.

**Contents:**
- Three dropdown input cells:
  - Entry age (25 / 30 / 35 / 40 / 45)
  - Mortality stress factor (0.75 to 1.25 in 0.05 steps)
  - Interest rate (5.0% to 8.0% in 0.5% steps)
- Live mortality table: qx_stressed = qx_base × stress factor, with 
  full recursive life table (lx, dx, px, tpx) and pricing chain 
  (annuity factor, TA factor, EPV components, gross premium) 
  recalculating automatically on dropdown change
- Mortality stress results table (one-variable Data Table, column 
  input = stress factor): stressed premium, ΔP vs base, % change 
  for all stress levels simultaneously
- Interest rate stress results table (one-variable Data Table, column 
  input = interest rate): premium at each rate, ΔP vs base (H28), 
  % change — base anchored to live H28 so interest rate sensitivity 
  is isolated cleanly regardless of current mortality stress setting
- Bar chart: stressed premiums with base scenario highlighted in 
  contrasting colour for visual identification
- Conclusion paragraph: mortality vs interest rate sensitivity 
  comparison with quantified findings

## Tools
Microsoft Excel — equivalence principle pricing, recursive life table 
construction, Data Tables for scenario analysis















