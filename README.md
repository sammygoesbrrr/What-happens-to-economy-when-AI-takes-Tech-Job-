# AI, Tech Displacement, and Consumer Demand

An agent-based model of what happens after a tech firm adopts AI: which tasks move to AI, who
gets fired, how long their savings last, where they go next, and what that does to consumer
demand in the rest of the economy.

Built from a hand-drawn flowchart, formalised with a task-based production model and an
agent-based savings model, and calibrated where possible against US data.

## What the model does

1. **Firm side.** Work is split into tasks ordered junior → senior. AI takes a task if it is
   technically able and cheaper per unit. The firm has a fixed budget, decides headcount once a
   year using *last year's* AI cost, and fires in one month.
2. **Worker side.** Fired workers keep spending at their old level and live off savings while
   searching. When savings run out, they take a lower-paying non-tech job and never return.
3. **Demand side.** Switchers cut spending; they also crowd into service jobs and push that wage
   down. Money the firm no longer pays in wages goes to AI providers, whose owners spend a much
   smaller share of it.
4. **Long run (to 2040).** New tasks can be created, service wages respond, and lower demand can
   feed back into firm budgets.

## Data

| Input | Source |
|---|---|
| Tech employment | FRED `CES6054150001` (BLS CES, computer systems design) |
| Token price at fixed coding capability | Epoch AI, HumanEval benchmark |
| Personal saving rate | FRED `PSAVERT` |
| CPI deflator | FRED `CPIAUCSL` |
| Marginally attached workers | FRED `LNU05026642` |
| Wages, service employment | BLS OEWS May 2025, hand-extracted |
| Aggregate demand (optional) | FRED `PCE` |

## Assumptions

Every parameter is tagged **DATA**, **HAND** (typed from a BLS table), or **PLACEHOLDER**.
Results that depend on a placeholder are reported as scenarios, never as findings.

**Structural assumptions (not from any paper):**
- Firms decide headcount yearly using last year's AI cost, so a cost change is acted on with a
  one-year lag.
- Tasks given to AI stay with AI unless fired workers are still searching and willing to
  return to the same company. Also, if the company hires employee before a year.
- The firm's task budget is fixed within the year. This is what connects AI cost to employment;
  without it, a token-cost increase cannot cause firing.
- Displaced workers keep spending at their old level until savings run out.(Though it doesn't really happen in real
  life, but let's assume they are hopeful that they will get a job within a month or two,
  so their lifestyle doesn't see any major change).
- All tasks are imagined at same level of cost/important.
  So junior techy senior techy doesn't matter here (another wrong assumption, as most fired people are jumior techy)

**Key placeholders (no public data source):** tokens needed per task, the elasticity of
substitution between tasks (σ), the new-task creation rate, income while searching, the spread of
savings rates across workers, and all spending-category values.

**The token-cost direction is scenario, not data.** At fixed coding capability, the token price
fell roughly 375× in sixteen months in the uploaded data. AI cost per task can only rise if
tokens-per-task rises faster than price falls, and no public dataset measures that. Three
scenarios are run: price-driven fall, offsetting, and rising.

## Core results

Model behaviour under the stated assumptions. **Not forecasts.**

- **Jobs recover; people do not.** Tech jobs dip after the firing wave and return close to their
  starting level, because AI lets the fixed budget buy more output (in the beginning) and new tasks add labour
  work. Consumer spending never recovers: workers switched out within weeks, and the returning
  jobs go to new entrants. The damage outlives its cause.
- **The savings buffer is short.** Using the national saving rate, the median worker can fund
  roughly 1.3 months of search.(This is also a wrong approach, but we did it for this model) Income during search therefore matters more than accumulated
  savings, and is currently the model's weakest input (No data there).
- **Who leaves is decided by savings, not skill.** Firing is random by construction, yet the
  workers who leave the industry are drawn from the bottom of the savings distribution: their
  buffer runs out first.
- **Around half the demand loss falls on people never employed in tech.** Switchers crowding into
  service jobs push service wages down, and that loss is comparable in size to the switchers' own.
- **The "token cost rises → firing" arrow holds only if σ < 1.** At σ = 1 a token-cost rise has
  no employment effect; above 1 it raises employment. A rising AI cost also *prevents* new
  automation, which can dominate the effect on already-automated tasks. (It's I think a wrong result assumption led to,
  no employee goes back to his earlier office, and all firms are cutting employee down, and usually they calculate revenue yearly,
  so by the tome a compaany checks it's revenue, it's already too late, for the employee to get back into tech industry). 
- **No spiral without a strong feedback.** The multiplier converges to a lower but stable level.
  A true downward spiral requires demand losses to shrink firm budgets strongly enough to cause
  further firing; the strength needed is tested, not assumed.

## Running it

Colab notebook, cells are standalone: Cell 0 uploads data, Cell 1 writes the model file, Cell 6
runs the Monte Carlo (3 σ values × 3 cost scenarios × 30 runs) and caches to disk. Later cells
load cached results and rerun automatically if a parameter changes.

## Limitations

- Tracks one fixed cohort: once they have switched, no further displacement occurs, which is why
  the spending loss flattens.
- New graduates who are never hired are not counted anywhere, though they are most exposed to
  junior-task automation.
- Uncertainty is currently within-scenario only. The parameters that most drive the outcome
  (σ, new-task rate, income while searching) are set by hand and swept as scenarios.
- Not validated. Comparisons against actual employment are plausibility checks.

## References

- Acemoglu, D. & Restrepo, P. (2018). The race between man and machine. *AER* 108(6), 1488–1542.
  — task allocation, eqs (1), (5), (6); new tasks and long-run correction.
- Applegate, J. M. & Janssen, M. A. (2022). Job mobility and wealth inequality.
  *Computational Economics* 59, 1–25. — savings as a share of wage, agent-based design, figure styles.
- Pissarides, C. A. (2000). *Equilibrium Unemployment Theory*. — job finding. *(unverified at source)*
- Kaldor, N. (1956); Kahn, R. F. (1931). — worker vs owner spending, multiplier. *(unverified at source)*
- Geary, R. C. (1950); Stone, R. (1954). — linear expenditure system. *(unverified at source)*
