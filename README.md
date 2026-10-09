# CarbonIQ: Business Sustainability Intelligence

A carbon footprint tracker for a multi-site business: it turns energy, fuel, travel and waste data into tonnes of CO2e, splits them into Scope 1, 2 and 3, compares facilities, forecasts the next 12 months, tests whether a 30% reduction target is reachable with a set of levers, and writes a short sustainability summary with Groq.

Built in Google Colab with Groq (`openai/gpt-oss-120b`).

> **The data is made up and the emission factors are assumptions.** Five facilities over 24 months were generated, and the factors (electricity 0.71 kg CO2e per kWh, diesel 2.68 per litre, gas 2.0 per cubic metre, air travel 0.15 per km, waste 0.5 per kg) are rough values you can edit. The notebook also accepts your own `carbon_data.csv`.

## What it does

1. **Calculates emissions** from activity data and splits them into Scope 1 (diesel and gas), Scope 2 (electricity) and Scope 3 (business travel and waste only, so it is partial).
2. **Compares facilities** by total, per employee and per million of revenue, and says plainly when the gap is too small to rank them.
3. **Tests three forecast methods on the last 6 months** (same month last year, a straight trend line, trend plus month effects) and uses the best one for the next 12 months.
4. **Tests a 30% reduction target** against five levers with assumed reduction percentages.
5. **AI sustainability summary (Groq).** The AI only gets calculated numbers, is told not to claim any action is already happening, and must say that the lever percentages are assumptions, Scope 3 is partial and the data is made up.

## Results from the run

| Measure | Result |
|---|---|
| Data | 5 facilities x 24 months, 12,284.6 t CO2e in total |
| Scope split (24 months) | Scope 1: 23.1%, Scope 2: 66.2%, Scope 3: 10.7% (partial) |
| Year 1 vs Year 2 | 6,274.8 t vs 6,009.9 t (-4.2%) |
| Current footprint (last 12 months) | 6,009.9 t CO2e |
| Highest-emitting facility | Pune Plant, 3,647.6 t over 24 months |
| Emissions per employee | 0.63 to 0.66 t, a spread of only 4.8%, so no facility is clearly better |
| Forecast, next 12 months | 5,745 t (trend + month effects) |
| Forecast error on last 6 months | trend + month effects **2.45%**, same month last year 5.14%, straight trend 5.60% |

**Reduction target: -30% means 4,206.9 t.**

| Lever (assumed reduction) | Saving | Share of baseline |
|---|---|---|
| Solar + LED (electricity -25%) | 992.7 t | 16.5% |
| EV / CNG fleet (diesel -30%) | 283.3 t | 4.7% |
| Virtual meetings (travel -40%) | 149.6 t | 2.5% |
| Waste recycling (waste -35%) | 99.6 t | 1.7% |
| Gas efficiency (gas -15%) | 65.4 t | 1.1% |
| **All levers together** | | **26.5%** |

**The plan falls 3.5 points short of the 30% target.** The notebook reports the gap instead of rounding it away, and the AI summary recommends modelling extra levers and validating the assumptions with pilots.

## What the evaluation showed

- **Electricity dominates.** Scope 2 is two thirds of the footprint, so the single biggest lever (Solar + LED) delivers about 16.5 points on its own. This depends on the assumed electricity factor.
- **A tested forecast beats a fancy one.** Three simple methods were compared on held-out months before one was chosen.
- **Facility rankings can be meaningless.** A 4.8% difference in emissions per employee is too small to call anyone best or worst.
- **An earlier version overclaimed.** The AI summary said levers were already "being deployed" and called 24 months of emissions the "current footprint". The prompt now uses the last 12 months and states that the levers are only scenarios.

## Limitations

- **Made-up data and assumed factors.** Do not quote the totals as a real company's footprint.
- **Scope 3 is partial.** Real Scope 3 (purchased goods, transport, use of products) is usually much larger than travel and waste.
- **The forecast was chosen on a 6-month test window**, which is short. It also misses year-on-year cycles that two years of data cannot show.
- **Lever percentages are assumptions and are simply added up.** Real levers overlap, cost money and take time.

## Tech stack

pandas, scikit-learn, matplotlib, seaborn, Groq API.

## How to run

1. Open the notebook in Google Colab.
2. Run the cells from top to bottom.
3. When asked, paste your own Groq API key (free at console.groq.com). It is hidden while typing and is never saved in the notebook.

## Files the notebook creates

- `carboniq_emissions.csv`: monthly emissions per facility by scope
- `carboniq_levers.csv`: the five levers with savings
- `carboniq_forecast.csv`: the 12-month forecast

---

Built by **Akshat Kesharwani** | [GitHub](https://github.com/akshatkesharwani-info) | [Portfolio](https://akshatkesharwani-info.github.io/)
