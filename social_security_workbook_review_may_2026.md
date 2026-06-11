# Review of `Breakeven_cola_fix.xlsx`

I reviewed the structure and formulas in the workbook, especially the `Main` sheet where the Social Security claiming comparisons are being modeled.

Overall, the workbook is becoming a solid retirement decision model rather than just a simple Social Security breakeven calculator. The addition of COLA, discount rate, and investment return assumptions significantly improves the realism of the analysis.

A few observations and suggestions stood out.

---

# What Looks Correct

## 1. FRA benefit scaling table (Rows 19–28)

The percentage multipliers for claiming ages 62–70 appear reasonable:

- 62 = 70%
- 67 = 100%
- 70 = 124%

This correctly reflects delayed retirement credits after FRA.

The year-to-year change calculations also appear correct.

---

## 2. COLA growth logic (Rows 32–71)

The revised COLA handling is substantially improved.

The workbook now correctly:

- Starts benefits at the chosen claim age
- Applies annual COLA increases after claiming
- Allows comparison between different claim ages

This fixes the earlier conceptual problem where delayed claimants were not receiving the same inflation-adjusted growth path.

The formulas like:

```excel
=B34*(1+$B$13)
```

are appropriate for compounding COLA year over year.

The initialization formulas for delayed claim ages also appear conceptually correct:

```excel
=B25*(1+B13)^(A25-A20)
```

This properly carries inflation adjustments forward before the delayed claimant starts receiving payments.

That is an important improvement.

---

## 3. Lifetime cumulative payout tables (Rows 77+)

The cumulative totals are structured correctly.

For example:

```excel
=C79+B80
```

properly accumulates total benefits received over time.

This makes the crossover analysis easier to visualize.

---

# Important Conceptual Issue

## Present value columns are not economically correct yet

Columns H–J are labeled as:

- “62 total in today’s dollars”
- “67 total in today’s dollars”
- “70 total in today’s dollars”

However, the formulas currently discount the cumulative totals directly.

Example:

```excel
=C90/(1+$B$15)^(A90-$A$78)
```

This discounts the *entire accumulated total* at once.

Economically, that is not the same as discounting each yearly payment individually.

The mathematically correct method is:

1. Discount each year’s benefit payment back to present value
2. Then accumulate the discounted values

Instead of:

```excel
Present Value of cumulative total
```

you want:

```excel
Cumulative total of discounted yearly payments
```

This distinction matters a lot.

Especially when comparing:

- early claiming
- delayed claiming
- investment return assumptions
- inflation assumptions

The current method slightly overstates the value of delayed strategies.

---

# Suggested Improvement for Present Value Columns

A better structure would be:

## Add helper columns:

### Example for age 62 strategy

#### Annual payment

Already exists:

```excel
=B79
```

#### Discounted annual payment

```excel
=B79/(1+$B$15)^(A79-$A$78)
```

#### Running discounted cumulative total

```excel
=PreviousRow + CurrentDiscountedPayment
```

This produces a true Net Present Value style comparison.

That would significantly improve the financial realism of the workbook.

---

# Extremely Important Missing Factor

## Survivor benefit analysis

For married retirees, delayed claiming can dramatically affect the surviving spouse.

This is one of the biggest reasons many higher earners delay Social Security.

Your workbook currently focuses primarily on:

- personal lifetime breakeven
- cumulative payout

But the largest real-world effect may be:

- increasing the surviving spouse’s guaranteed inflation-adjusted income floor

Especially:

- if one spouse earned substantially more
- if longevity differs
- if pensions are limited

A future enhancement could model:

- primary earner dies first
- survivor inherits larger SS payment
- compare survivor income under different claiming ages

That changes the decision substantially.

---

# Another Major Missing Factor

## Taxation of Social Security benefits

At higher retirement income levels:

- up to 85% of Social Security becomes taxable

Your workbook currently compares gross benefits.

But actual spendable income can differ significantly.

Especially if:

- RMDs begin
- IRA withdrawals rise
- investment income grows

This can create surprising situations where:

- delaying SS lowers long-term taxes
- or increases IRMAA Medicare surcharges

Adding a simplified tax model later would greatly strengthen the workbook.

---

# Strong Recommendation

## Add a “Portfolio Drawdown Before SS” model

This is probably the single most useful enhancement you could make.

Right now the workbook compares:

- SS payout totals

But many retirees actually ask:

> “Should I spend investments first while delaying Social Security?”

That requires modeling:

- retirement account balance
- withdrawals before claiming
- investment returns
- reduced withdrawals after claiming
- sequence of returns risk

This is where the real optimization happens.

The workbook already has the beginnings of this idea with:

- Retirement Funds
- Rate of Return

but it is not fully integrated yet.

---

# Suggested New Outputs

These would dramatically improve interpretability.

## 1. “Age when delayed strategy permanently wins”

Not just crossover.

A strategy can cross over temporarily.

Better:

- identify the age after which delayed claiming never falls behind again.

---

## 2. “Required lifespan to justify delay”

Display:

- probability of reaching crossover age
- based on SSA actuarial tables

This turns the workbook from:

- deterministic

into:

- probabilistic decision support.

---

## 3. Inflation-adjusted income floor chart

This would visually show:

- guaranteed income over time
- under each claiming strategy

Very powerful psychologically.

---

## 4. Portfolio depletion age

Especially useful if delaying SS requires spending savings first.

Show:

- when retirement assets hit zero
- under each strategy

This is often more important than total lifetime payout.

---

# Technical Suggestions

## Use named ranges

You now have enough assumptions that named ranges would improve readability.

Examples:

- FRA_Benefit
- COLA
- DiscountRate
- ReturnRate
- RetirementFunds

This makes formulas much easier to audit.

---

## Consider converting ranges into Excel Tables

Benefits:

- cleaner formulas
- auto-expansion
- easier charting
- fewer reference mistakes

---

## Add conditional formatting

Ideas:

- highlight crossover year
- highlight best NPV strategy
- highlight portfolio depletion

This would make the workbook much easier to interpret visually.

---

# Overall Assessment

The workbook is now beyond a simple Social Security breakeven calculator.

You are approaching a fairly sophisticated retirement income modeling framework.

The most important next improvements would probably be:

1. Correct present value calculations
2. Integrate investment drawdown modeling
3. Add survivor-benefit analysis

Those three additions would move the workbook much closer to the types of analysis done in professional retirement planning software.

The structure is already good enough to support those additions without major redesign.
