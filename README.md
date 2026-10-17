<p align="center">
  <img src="https://github.com/user-attachments/assets/bb3d8e3c-4e34-4690-a53a-bf148d650cf0" alt="Project overview" style="max-width:100%; width:900px; height:auto;" />
</p>

# Hospitality Dashboard — Hotel Booking Analysis

A data analytics project that transforms three years of hotel booking records into actionable business intelligence using **SQL Server** and **Power BI**. The analysis covers revenue trends, occupancy, Average Daily Rate (ADR), cancellations, and market segment performance.

---

## Business Problem

Hotel management needs to know whether revenue is growing year over year and which factors — hotel type, market segment, seasonality, and cancellations — are driving or limiting that growth. Decisions on pricing strategy, parking capacity, and marketing channel investment all depend on a reliable, structured view of the data.

---

## Objective

- Determine whether hotel revenue is growing year over year.
- Break down revenue by hotel type, market segment, and season.
- Measure the financial impact of cancellations on realised revenue.
- Track Average Daily Rate (ADR) trends over time.
- Assess parking lot utilisation to inform infrastructure decisions.
- Deliver an interactive Power BI dashboard for stakeholder use.

---

## Dataset

| File | Description |
|------|-------------|
| `hotel_revenue_historical_Dataset.xlsx` | Three year-level sheets (2018, 2019, 2020) of hotel booking records |
| Sheet: `market_segment$` | Market segment discount lookup table |
| Sheet: `meal_cost$` | Meal type cost-per-night lookup table |

**Key columns:** `hotel`, `is_canceled`, `arrival_date_year`, `arrival_date_month`, `stays_in_weekend_nights`, `stays_in_week_nights`, `adr`, `market_segment`, `meal`, `required_car_parking_spaces`, `country`, `customer_type`

> Source: Publicly available hotel revenue historical dataset (Excel/XLSX format).

---

## Tools

| Tool | Purpose |
|------|---------|
| SQL Server (SSMS) | Data consolidation, enrichment, and exploratory analysis |
| Power BI Desktop | Interactive dashboard and KPI visualisation |
| DAX | Calculated measures inside Power BI |
| Excel (XLSX) | Source data format |

---

## Data Cleaning and Preparation

All cleaning and preparation is handled in SQL before the data reaches Power BI.

- **Consolidate three years** using `UNION ALL` — not plain `UNION` — to preserve all booking records including any identical rows across years.
- **Handle NULL discounts and meal costs** with `COALESCE(..., 0)` on both lookup joins so NULL values do not propagate through revenue calculations.
- **Exclude cancelled bookings from revenue** with a `CASE WHEN is_canceled = 1 THEN 0` guard so only realised revenue is counted.
- **Derive key calculated columns:** `total_nights`, `total_guests`, `revenue`, `net_revenue`.

**Revenue formula:**
```
revenue = (stays_in_weekend_nights + stays_in_week_nights) x ADR x (1 - Discount)
net_revenue = revenue - (total_nights x meal_cost_per_night)
```

---

## Analysis and KPIs

| # | Business Question | Approach |
|---|-------------------|----------|
| Q1 | Is revenue growing year over year? | Annual revenue aggregation + `LAG()` window function for YoY growth % |
| Q2 | Which hotel type generates more revenue? | Revenue grouped by hotel and year |
| Q3 | How is ADR trending? | Average ADR and total nights by hotel and year |
| Q4 | What is the cancellation rate? | Cancellations as % of total bookings by hotel and year |
| Q5 | Which market segments perform best? | Revenue, ADR, and cancellation rate by segment |
| Q6 | Is parking capacity being utilised? | Parking spaces required as % of total nights stayed |
| Q7 | What does seasonality look like? | Monthly booking volume and revenue by hotel |

---

## Key Insights

> *These insights are derived from the SQL analysis of 2018–2020 booking data.*

- **Revenue growth:** Revenue grew from 2018 to 2019. The 2020 figures are partial (data covers only part of the year) and should not be used to draw a full-year trend conclusion.
- **Hotel type:** Resort Hotels generate a higher ADR than City Hotels. City Hotels drive higher booking volume across all years.
- **Cancellations:** City Hotels show a consistently higher cancellation rate than Resort Hotels, which directly reduces their realised revenue despite higher total bookings.
- **Market segments:** Online Travel Agencies (OTA) generate the highest booking volume but also the highest cancellation rates. Direct and Corporate segments deliver higher booking quality with lower cancellation rates.
- **Parking utilisation:** Required parking spaces represent a small fraction of total nights stayed, indicating the current parking capacity is adequate and does not need expansion.
- **Seasonality:** Resort Hotels peak in July and August. City Hotels show a more even distribution across the year, reflecting business travel demand.

---

## Recommendations

1. **Tighten deposit requirements for OTA bookings** to reduce the high cancellation rate from that channel and protect realised revenue.
2. **Apply dynamic pricing at Resort Hotels in peak summer months** (July–August) to maximise ADR when demand is highest.
3. **Invest in growing Direct and Corporate channels** — these segments deliver better booking quality with lower cancellation exposure.
4. **Do not expand parking infrastructure** based on current utilisation data; the cost is not justified by demand.
5. **Treat 2020 as a partial-year anomaly** in any long-term trend analysis given incomplete data coverage.

---

## Project Structure

```
Hospitality-Dashboard-Hotel-Booking-Analysis/
├── Hotel_Booking_Analysis.sql              <- All SQL queries (consolidation + 7 analysis queries)
├── Hotels_Booking_Analysis_Dashboard.pbix  <- Power BI interactive dashboard
├── hotel_revenue_historical_Dataset.xlsx   <- Source data (2018, 2019, 2020 + lookup tables)
└── README.md
```

---

## How to Run

### SQL

1. Open **SQL Server Management Studio (SSMS)**.
2. Import the Excel sheets as separate tables in your database:
   - `2018$`, `2019$`, `2020$` — booking data
   - `market_segment$` — discount lookup
   - `meal_cost$` — meal cost lookup
3. Open `Hotel_Booking_Analysis.sql`.
4. Run the **STEP 1 / STEP 2 / FINAL QUERY** block to generate the full enriched dataset used by Power BI.
5. Run individual **Q1–Q7 blocks** separately to explore specific business questions.

### Power BI

1. Open `Hotels_Booking_Analysis_Dashboard.pbix` in **Power BI Desktop**.
2. Go to **Transform Data > Data Source Settings** and update the SQL Server connection to your local instance.
3. Click **Refresh** to load data from the updated SQL query.
4. Use the dashboard slicers to filter by year, hotel type, and market segment.

---

## Author

Made with love by **Rakhi Yadav**

*Turning hotel booking data into meaningful business insights.*
