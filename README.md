# capstone-hosp (Prepared by Kelvin Chu)

# Hotel Booking Cancellations
---

## Problem statement

Hotels lose revenue when guests cancel their bookings. A cancelled room that is not resold earns nothing. Cancellations also make it hard to plan staff, supplies and overbooking levels. Hotel managers need to know when and why guests cancel and which new bookings are likely to cancel.

This project uses historical booking data from a City Hotel and a Resort Hotel. It addresses the problem in 2 parts:

- **Descriptive analytics:** Python analysis and a Power BI dashboard that show booking trends, cancellation patterns and the revenue at risk.
- **Predictive analytics:** a machine learning model that flags bookings with a high risk of cancelling at the time of booking.

Together they help managers set deposit rules, plan overbooking and contact high-risk guests early.

## Research objectives

1. **Descriptive:** Measure booking volume, cancellation rate and average daily rate by hotel, month and season.
2. **Descriptive:** Identify the booking factors linked to higher cancellation rates. These include lead time, deposit type, market segment, distribution channel, customer type and guest history.
3. **Descriptive:** Estimate the revenue lost to cancellations and present the findings in an interactive Power BI dashboard for hotel managers.
4. **Descriptive:** Compare average daily rate and length of stay between the City Hotel and the Resort Hotel across the year.
5. **Descriptive:** Examine how guest profile and history relate to cancellations. These include repeat guests, previous cancellations, special requests and country of origin.

## Data understanding

The 5 files were loaded in `clean.ipynb` straight from this repository. Each was checked for structure, missing values, duplicates and odd values.

- `Bookings.csv` is the main table with 1 row per booking. The other 4 files are lookup tables joined by ID columns. Every ID has a match.
- Bookings cover arrivals from July 2015 to August 2017 at 2 hotels.
- 4 columns have missing values: `children` (4), `country` (488), `agent` (16,340) and `company` (112,593).
- 180 bookings have no guests. 2 have an impossible average daily rate (ADR) of below 0 or 5,400.
- 33,010 bookings match another booking in every column except the ID. These are likely group bookings with several rooms.

The full data issues log is in `clean.ipynb`.
