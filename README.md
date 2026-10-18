# capstone-hosp

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
4. **Predictive:** Build and compare at least 3 classification models that predict whether a booking will cancel. Measure them against a simple baseline.
5. **Predictive:** Explain the main drivers of cancellation from the best model. Propose how it can be deployed, monitored and retrained.

## Data

Sample data provided by NTUC LearningHub.

| File | Rows | What it holds |
| --- | --- | --- |
| `Bookings.csv` | 119,390 | 1 row per booking |
| `Hotels.csv` | 2 | City Hotel and Resort Hotel |
| `Market Segments.csv` | 26 | Market segment and distribution channel |
| `Customer Types.csv` | 36 | Customer type, deposit type and meal plan |
| `Reservation Status.csv` | 3 | Check-Out, Canceled or No-Show |

## Project files

| File | Purpose | Status |
| --- | --- | --- |
| `01_clean.ipynb` | Understand and clean the data | In progress |
| `02_eda.ipynb` | Descriptive analysis | Not started |
| `03_model.ipynb` | Predictive model | Not started |
| `hotel_dashboard.pbix` | Power BI dashboard | Not started |
| `hotel_slides.pptx` | Final presentation | Not started |

## Sections to be added

- Data understanding and preparation
- Exploratory data analysis
- Power BI dashboard
- Predictive model and results
- Recommendations
- Conclusion and limitations
- Tools and resources
- Retrospective

## Tools

Python (pandas, matplotlib, seaborn, scikit-learn) in Google Colab - Microsoft Power BI Desktop - GitHub
