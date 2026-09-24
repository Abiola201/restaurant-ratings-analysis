# Restaurant Ratings Analysis — Digitaley Drive Data Analyst Bootcamp Capstone

A Power BI analysis of a 2012 Mexican restaurant consumer survey, built to help entrepreneurs and investors decide where and how to invest in a restaurant.

**Tool:** Microsoft Power BI (Power Query, DAX, data modeling)
**Dataset:** Restaurant Rating consumer survey, Mexico — 138 consumers, 130 restaurants, 1,161 ratings

---

## Executive Summary

This project analyzes a 2012 consumer survey of restaurants across four Mexican cities to help entrepreneurs and investors make more informed decisions about where and how to invest in a restaurant. The dataset covers 138 consumers, 130 restaurants, and 1,161 individual ratings, linked through consumer preferences and restaurant cuisine tags.

A five-page Power BI dashboard was built to answer four questions: what distinguishes the highest-rated restaurants, who the survey respondents are and whether the sample is biased, where gaps exist between what consumers want and what restaurants supply, and what an investor should look for.

**Headline findings:**
- Restaurants average 1.20 out of 2 overall, with 42% of ratings "Highly Satisfactory" and 22% "Unsatisfactory." Service is the weakest component (1.09) compared with food (1.22).
- Restaurant-level features move ratings only modestly — price point and enclosed seating help a little — while differences between types of consumers are larger. Matching a diner's preferred cuisine does not raise their rating.
- The survey sample is heavily skewed toward young, single, budget-conscious students, so findings describe that segment of diners, not the broader Mexican dining market.
- Several cuisines with real consumer demand — including Hot Dogs, Latin American, and Afghan — have no restaurants serving them at all, while cuisines like Mexican are demanded far more than the current supply of restaurants can match.

The full write-up, including methodology and detailed findings, is in [`Restaurant_Rating_Capstone_Report.docx`](./Restaurant_Rating_Capstone_Report.docx).

---

## Project Overview
![Overview](Screenshot%202026-09-24%20203716%20Overview.png)
This capstone was assigned as part of the Digitaley Drive Data Analyst Bootcamp. The brief frames the analyst as contracted to analyze a restaurant rating dataset and draw out insight that would help business entrepreneurs and investors make more informed decisions. Four guiding questions were set:

1. What can be learned from the highest-rated restaurants? Do consumer preferences affect ratings?
2. What are the consumer demographics, and does this indicate bias in the sample?
3. Are there demand and supply gaps that can be exploited in the market?
4. If investing in a restaurant, which characteristics should be prioritized?

The deliverable is a five-page interactive Power BI dashboard — **Overview, Ratings Drivers, Consumers, Demand and Supply, and Investment** — each built around one of these questions.

---

## Data Sources

The dataset is a 2012 consumer survey of restaurants in Mexico, provided as five linked tables:

| Table | Rows | Contents |
|---|---|---|
| Consumers | 138 | Demographics: age, city, budget, marital status, occupation, drinking and smoking habits, transportation |
| Restaurants | 130 | Attributes: city, price tier, alcohol service, smoking policy, parking, area, franchise status |
| Ratings | 1,161 | Overall, food, and service scores (0–2) linking a consumer to a restaurant |
| Consumer_Preferences | 328 (after de-duplication) | Cuisines each consumer says they prefer |
| Restaurant_Cuisines | 112 | Cuisines each restaurant is tagged as serving |

Restaurants span four cities: San Luis Potosí (84), Ciudad Victoria (23), Cuernavaca (21), and Jiutepec (2).

---

## Data Cleaning & Transformation

Cleaning and modeling were done in Power Query:

- **"None" preserved as a category.** In `Alcohol_Service` (87 rows) and `Parking` (65 rows), "None" is a genuine answer meaning no alcohol or no parking is offered — not a missing value — and was kept as text.
- **Blank consumer fields relabeled "Unknown."** 39 blanks across `Smoker`, `Transportation_Method`, `Marital_Status`, `Children`, `Occupation`, and `Budget` were set to "Unknown" rather than dropped, so the consumer (and their ratings) stayed in the analysis.
- **Duplicate rows removed.** `Consumer_Preferences` had 2 exact duplicate rows (330 → 328); `Restaurant_Cuisines` was also de-duplicated.
- **Constant and empty columns dropped.** `Country` (always "Mexico") and `Zip_Code` (blank for 74 of 130 restaurants) were removed.
- **Data model.** `Ratings` sits at the center of a star schema, linked to `Consumers` and `Restaurants`. A shared `Cuisine` dimension table links `Consumer_Preferences` and `Restaurant_Cuisines`, so consumer demand and restaurant supply can be compared cuisine by cuisine.
- **Calculated columns and measures.** A `Cuisine_Match` column flags whether a rated restaurant's cuisine overlaps with the consumer's stated preferences. DAX measures were built for average ratings, % "Highly Satisfactory," demographic shares, and a `Market Gap` classification (Unserved demand / Under-supplied / Balanced / Over-supplied / Niche demand).
- **Known limitation.** Only 95 of 130 restaurants (73%) carry a cuisine tag, so supply-side figures in the Demand and Supply analysis are a partial picture.

---

## Analysis

### Q1 — What distinguishes the highest-rated restaurants? Do preferences affect ratings?
![Ratings drivers](Screenshot%202026-09-24%20203818%20Rating%20drivers.png)
Across 1,161 ratings, the overall average is 1.20/2 (Food 1.22, Service 1.09), with 42% "Highly Satisfactory" and 22% "Unsatisfactory."

- **Price:** Low 1.07 vs. Medium 1.25 / High 1.26
- **Area:** Closed/enclosed 1.21 vs. Open 1.12
- **Alcohol service:** Full Bar 1.26, Wine & Beer 1.24, None 1.17
- **Cuisine (20+ ratings):** International (1.51) and Japanese (1.34) rate highest; Italian (1.04) and Burgers (1.03) rate lowest. Mexican sits at 1.19.

Consumer-side differences are larger: Social drinkers (1.34) vs. casual drinkers (1.02); consumers with young kids (0.87) vs. independent adults (1.22); high-budget (1.48) vs. low-budget (1.14) diners.

**Preference matching does not raise ratings** — matched ratings average 1.11 vs. 1.25 for unmatched (Mann-Whitney p ≈ 0.03), a small, unexpected difference read as "no evidence preference-matching helps."

Top-rated restaurants (min. 5 ratings): Las Mañanitas, Emilianos, and Michiko Restaurant Japonés each average 2.00/2, though on samples of only 5–8 ratings.

### Q2 — Who are the consumers? Is the sample biased?
![Consumers](Screenshot%202026-09-24%20203905%20consumers.png)
- 86% of those reporting an occupation are students
- 80% are 25 or younger (101 of 138 in the 21–25 bracket alone)
- 92% of those reporting marital status are single
- 70% report a medium budget; only 5 people (4%) report a high budget

The sample represents young, single, budget-conscious diners — largely students — not the broader Mexican dining public. Small subgroups (2 unemployed consumers, 5 high-budget consumers) are too small for firm conclusions.

### Q3 — Are there demand and supply gaps?
![Demand and supply](Screenshot%202026-09-24%20203955%20Demand%20and%20supply.png)
| Cuisine | Consumers preferring | Restaurants serving | Classification |
|---|---|---|---|
| Mexican | 97 | 28 | Under-supplied |
| Coffee Shop | 8 | 1 | Under-supplied |
| Family | 8 | 2 | Under-supplied |
| Hot Dogs | 6 | 0 | Unserved demand |
| Latin American | 6 | 0 | Unserved demand |
| Afghan | 4 | 0 | Unserved demand |
| Bar | 3 | 13 | Over-supplied |

Mexican cuisine is the most-demanded category (97 of 138 consumers) yet remains under-supplied. A parallel gap exists on price: 35% of restaurants are low-priced against 25–27% of consumers with a low budget, while 19% of restaurants are high-priced against just 4% of consumers with a high budget.

### Q4 — What should an investor look for?
![Investment](Screenshot%202026-09-24%20204044%20Investment.png)
- **Price point:** Medium or high, not low
- **Setting:** Enclosed/closed-area rather than open-air
- **Location:** Cuernavaca (1.38 avg.) or San Luis Potosí (1.21) over Ciudad Victoria (0.93)
- **Cuisine:** An under-supplied category with proven demand — Mexican, Coffee Shop, Family, Bakery, Breakfast, or Regional
- **Franchise status** showed almost no effect (Yes 1.22 vs. No 1.20) and shouldn't weigh heavily

These are hypotheses drawn from a young, student-heavy sample of 138 respondents surveyed in 2012, not guarantees.

---

## Key Findings

1. **Service is the weak point** — average service ratings (1.09) trail food ratings (1.22) across the board.
2. **Who's rating matters more than restaurant features** — gaps between consumer segments are consistently larger than gaps between restaurant price tiers, areas, or alcohol policies.
3. **Preference-matching does not predict satisfaction** — consumers rate restaurants outside their stated cuisine preference no lower, and slightly higher.
4. **The sample is young, single, and budget-conscious** — 86% student, 80% aged 25 or under, 92% single.
5. **Mexican cuisine is the biggest visible opportunity** — most-demanded and under-supplied even accounting for incomplete tagging.
6. **Several niche cuisines have zero supply** — Hot Dogs, Latin American, and Afghan have real, if modest, demand and no restaurants tagged to serve them.

---

## Recommendations

- **For an investor or entrepreneur:** prioritize a medium-to-high-priced, enclosed-seating restaurant in a well-rated city (Cuernavaca or San Luis Potosí), in an under-supplied cuisine category such as Mexican, Coffee Shop, or Family dining.
- **For an existing restaurant operator:** invest in service quality specifically — it's the most common weak point across every price tier and cuisine.
- **For market entry:** the Hot Dogs, Latin American, and Afghan cuisine gaps are worth exploring further, though on small consumer counts (4–6 people) that would benefit from additional market research before committing capital.
- **For interpreting this analysis:** treat findings as representative of young, budget-conscious, largely student diners rather than the general Mexican dining public. A follow-up survey with a more age- and income-diverse sample would strengthen these conclusions.

---

## Repository Contents

- `Restaurant_Rating_Capstone_Report.docx` — full written report (this README condensed from it)
- `Restaurant_Rating_Capstone.pbix` — Power BI dashboard file
- `screenshots/` — dashboard page images (Overview, Ratings Drivers, Consumers, Demand and Supply, Investment)

---

*Prepared by Abiola Damilola Mercy for the Digitaley Drive Data Analyst Bootcamp capstone.*
