# Meta Ads Performance Dashboard: Facebook & Instagram

![Power BI](https://img.shields.io/badge/Power%20BI-dashboard-F2C811)
![DAX](https://img.shields.io/badge/DAX-measures-blue)
![Power Query](https://img.shields.io/badge/Power%20Query-ETL-green)

**Business question:** how are our Facebook and Instagram ad campaigns performing, which audiences and ad formats
engage the most, and where does the funnel lose people between the click and the purchase?

This end-to-end Power BI project turns 400,000 raw ad events into an interactive dashboard: data cleaning in
Power Query, a star-schema data model, DAX measures for the marketing KPIs, and one report page per platform.

![Facebook page of the dashboard](images/dashboard_facebook.png)

## Data
Four CSV files in [`data/raw/`](data/raw/), May to August 2025:

| Table | Rows | Content | Role in the model |
|---|---|---|---|
| `ad_events` | 400,000 | One row per event: impression, click, like, comment, share, purchase, with timestamp | Fact table |
| `ads` | 200 | Platform (Facebook / Instagram), ad type, targeted gender, age group and interests | Dimension |
| `campaigns` | 50 | Campaign name, start and end dates, duration, total budget | Dimension |
| `users` | 9,841 | Gender, age, age group, country, location, interests | Dimension |

## Data model
- **Star schema** built around `ad_events` (fact table) with `ads`, `campaigns`, `users` and a `Calendar Table` as
  dimensions, related through `ad_id`, `campaign_id`, `user_id` and the event date.
- **Calendar Table** for time analysis (month, week number, day name).
- **Field parameter** (`Select Dynamic Measures`) that lets the user switch the KPI shown in the trend charts.

## KPIs (DAX measures)

| KPI | Definition |
|---|---|
| Impressions, Clicks, Comments, Shares, Purchases | Count of events of each type |
| Engagement | Clicks + Comments + Shares |
| CTR | Clicks / Impressions |
| Engagement Rate | Engagement / Impressions |
| Conversion Rate | Purchases / Clicks |
| Purchase Rate | Purchases / Impressions |
| Total Budget, Avg. Budget per Campaign | Sum and average of campaign budgets |

## Dashboard
Two report pages with the same layout, **Facebook** and **Instagram** (switch with the buttons on the right), plus a
calendar tooltip page.
- 12 KPI cards (volumes and rates).
- Engagement by targeted gender (donut), by age (column chart) and by country (Azure map).
- Weekly and hourly engagement trends driven by the dynamic measure selector.
- Calendar view by month and a matrix comparing ad types, with conditional formatting.
- Slicers: dynamic measure, campaign name and target interest.

## Key results (Facebook, full period)

| Impressions | Clicks | Engagement | Purchases | CTR | Engagement Rate | Conversion Rate | Purchase Rate |
|---|---|---|---|---|---|---|---|
| 216.0K | 25.4K | 29.3K | 1.3K | 11.76% | 13.56% | 5.21% | 0.61% |

Instagram over the same period: 123.8K impressions, 14.7K clicks, 708 purchases, CTR 11.86%,
conversion rate 4.82%. Total budget across the 50 campaigns: 2.54M (51K per campaign on average).

**Ad formats (Facebook)**

| Ad type | Impressions | Clicks | CTR | Engagement Rate | Conversion Rate | Purchase Rate |
|---|---|---|---|---|---|---|
| Carousel | 47.8K | 5.6K | 11.73% | 13.44% | 5.05% | 0.59% |
| Image | 50.9K | 5.9K | 11.67% | 13.46% | 4.91% | 0.57% |
| Stories | 71.5K | 8.4K | 11.75% | 13.61% | **5.52%** | **0.65%** |
| Video | 45.8K | 5.4K | **11.88%** | **13.74%** | 5.22% | 0.62% |

## Insights
- **Strong top of funnel, weak bottom.** About 12% of impressions lead to a click, but only 5% of clicks end in a
  purchase: the main opportunity is after the click (landing page, checkout, offer).
- **Stories convert best and reach the most people:** highest impressions, conversion rate and purchase rate.
  **Video** has the highest CTR and engagement rate.
- **Audience:** ads targeting women generate 43% of engagement (all genders 35%, men 22%); engagement peaks between
  20 and 30 years old and drops sharply after 35.
- **Countries:** the United States, the United Kingdom, Canada, India and Germany generate the most engagement.
- **Timing:** engagement is spread evenly across the day (every hour within about 5% of the average), so the
  hour of delivery is not a strong lever in this data.

## Recommendations
1. Improve the post-click experience (landing page, checkout) to lift the 5% conversion rate.
2. Shift budget towards Stories (best conversion) and Video (best engagement).
3. Focus targeting on women aged 20 to 30 and on the top five countries.
4. Add retargeting for visitors who clicked but did not buy.

## Limitations
- The dataset appears to be synthetic (generated names and cities), so the insights illustrate the method rather
  than real market behavior.
- 225 user IDs in `users.csv` were converted to scientific notation by Excel (for example `1.20E+01`), so about 5%
  of events cannot be matched to a user profile. Audience breakdowns are computed on the matched events.

## Project structure
```
Power-BI-Project-Facebook-Instagram-Social-Media-Analysis/
├── dashboard/
│   └── meta_ads_performance_dashboard.pbix   Power BI report and data model
├── data/raw/                                 source CSV files
│   ├── ad_events.csv
│   ├── ads.csv
│   ├── campaigns.csv
│   └── users.csv
└── images/
    └── dashboard_facebook.png
```

## How to open
1. Install [Power BI Desktop](https://www.microsoft.com/power-platform/products/power-bi/desktop) (free, Windows).
2. Open `dashboard/meta_ads_performance_dashboard.pbix`. The data is already loaded in the file.
3. To refresh from the CSV files, point Power Query to your local `data/raw/` folder:
   **Home > Transform data > Data source settings > Change source**, then **Refresh**.

## Tools
Power BI Desktop, Power Query, DAX, data modeling (star schema), CSV.

## What I learned
This was my first end-to-end Power BI project. It covers data cleaning with Power Query, star-schema modeling,
DAX measures and calculated columns, KPI design, field parameters for dynamic visuals, and dashboard design for
business users.

## Author
**Hamza Charrad**, Data Analyst
[LinkedIn](https://www.linkedin.com/in/hamza-charrad/) · [Portfolio](https://hamzacharrad29-png.github.io/Portfolio/)
