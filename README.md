# Hotel Booking Cancellations - Capstone Project

**Prepared by:** Kelvin Chu

## Problem statement

Hotels lose revenue when guests cancel their bookings. A cancelled room that is not resold earns nothing. Cancellations also make it hard to plan staff, supplies and overbooking levels. Hotel managers need to know when and why guests cancel and how much revenue is at risk.

This project uses historical booking data from a City Hotel and a Resort Hotel. Python is used to clean and analyse the data. An interactive Power BI dashboard presents the findings so managers can explore bookings, cancellations and revenue by hotel, period and segment.

The findings help managers set deposit rules, plan overbooking and focus on the bookings most likely to cancel.

## Research objectives

1. Measure booking volume and cancellation rate by hotel, month and season.
2. Compare average daily rate and length of stay between the City Hotel and the Resort Hotel across the year.
3. Identify the booking factors linked to higher cancellation rates. These include lead time, deposit type, market segment, distribution channel and customer type.
4. Examine how guest profile and history relate to cancellations. These include repeat guests, previous cancellations, special requests and country of origin.
5. Estimate the revenue lost to cancellations and how far ahead of arrival guests cancel. Find the hotels, segments and months with the largest losses.

## 1. Load the data

The data is 5 CSV files provided by NTUC LearningHub for Project 14.1 Hotel Bookings. The files are stored in this repository. `hotel.ipynb` loads them straight from GitHub using `pd.read_csv` and each file's raw link. The notebook runs in Google Colab without Google Drive.

```python
bookings = pd.read_csv('https://raw.githubusercontent.com/ahchu2525/capstone-hosp/main/Bookings.csv')
```

| File | Rows | Columns | What it holds |
| --- | --- | --- | --- |
| `Bookings.csv` | 119,390 | 28 | 1 row per booking |
| `Hotels.csv` | 2 | 2 | City Hotel and Resort Hotel |
| `Market Segments.csv` | 26 | 3 | Market segment and distribution channel |
| `Customer Types.csv` | 36 | 4 | Customer type, deposit type and meal plan |
| `Reservation Status.csv` | 3 | 2 | Check-Out, Canceled or No-Show |

`Bookings.csv` links to the other 4 files through the ID columns `Hotel_ID`, `Market_Segment_ID`, `Customer_Type_ID` and `Reservation_ID`.

## 2. Understand the data

Each file was checked in `hotel.ipynb` for structure, missing values, duplicates and odd values.

- `Bookings.csv` is the main table with 1 row per booking. The other 4 files are lookup tables joined by ID columns. Every ID has a match.
- Bookings cover arrivals from July 2015 to August 2017 at 2 hotels.
- 4 columns have missing values: `children` (4), `country` (488), `agent` (16,340) and `company` (112,593).
- 180 bookings have no guests. 2 have an impossible average daily rate (ADR) of below 0 or 5,400.
- 33,010 bookings match another booking in every column except the ID. These are likely group bookings with several rooms.

### Data issues log

**Summary:** 14 issues were found. 3 are fixed by filling blanks, 4 by removing invalid rows, 3 by changing or adding columns and 4 are kept with a note on how they are used. 187 rows are removed in total (0.2% of bookings).

| No. | Issue | Rows | Plan |
| --- | --- | --- | --- |
| 1 | `children` is blank | 4 | Fill with 0 |
| 2 | `country` is blank | 488 | Fill with `Unknown` |
| 3 | `agent` is blank | 16,340 | Means no agent. Fill with 0 and add a `has_agent` column |
| 4 | `company` is blank | 112,593 | Too empty to use. Add a `has_company` column then drop `company` |
| 5 | No adults, children or babies | 180 | Remove. A booking with no guests is not valid |
| 6 | `adr` below 0 | 1 | Remove |
| 7 | `adr` of 5,400 (next highest is 510) | 1 | Remove as a typing error |
| 8 | `adr` of 0 | 1,959 | Keep. Many are free stays |
| 9 | 0 nights booked | 715 | Keep. These add 0 revenue |
| 10 | Meal plan has both `SC` and `Undefined` | 11,819 | Merge into `No meal` |
| 11 | Segment or channel is `Undefined` | 5 | Remove. Too few to analyse |
| 12 | Arrival date split over 3 columns with month as text | All | Build 1 `arrival_date` column |
| 13 | Rows identical except `Booking_ID` | 33,010 | Keep for the dashboard. Likely several rooms in 1 group booking. Remove before modelling |
| 14 | `reservation_status` gives away the answer to `is_canceled` | All | Use in the dashboard only. Never use in the model |

## 3. Clean the data

The 4 lookup tables were joined onto bookings. The issues in the data issues log were then fixed on a copy called `df_cleaned`.

- Filled blanks in `children`, `country` and `agent`. Dropped `company` after recording `has_company`.
- Removed 187 invalid rows: 180 with no guests, 2 with an impossible ADR and 5 with an Undefined segment or channel.
- Merged meal codes `SC` and `Undefined` into `No meal`.
- Built `arrival_date` and converted `reservation_status_date` to a date.
- Added 10 new columns such as `total_nights`, `room_revenue`, `season` and `cancel_days_before_arrival`.
- Final clean data: 119,203 rows and 40 columns.

## 4. Exploratory data analysis

All analysis uses the clean data. 37.1% of bookings were cancelled or did not show up. The City Hotel cancels more often (41.8%) than the Resort Hotel (27.8%).

1. **Volume and cancellation rate:** both hotels are busiest in summer. The Resort Hotel's cancellation rate more than doubles from 14.8% in January to 33.4% in August. The City Hotel stays between 37% and 46% all year.
2. **Price and length of stay:** the Resort Hotel's ADR rises from about 49 in January to 181 in August. City Hotel prices are steadier at 82 to 121. Resort guests stay 4.14 nights on average compared with 2.92 at the City Hotel.
3. **Booking factors:** cancellation rises from 9.6% for bookings made within 7 days to 67.7% for bookings made over a year ahead. 99.4% of Non Refund bookings cancel. Groups cancel most (61.1%) and Direct bookings least (15.4%).
4. **Guest profile and history:** repeat guests cancel 14.7% of the time compared with 37.8% for new guests. Guests who cancelled before cancel again 91.7% of the time. Guests with no special requests cancel 47.8% of the time compared with 22.0% with 1 request. Guests from Portugal cancel 56.7% of the time.
5. **Revenue lost:** cancelled bookings were worth 16.7 million against 26.0 million earned. Online travel agent bookings make up 10.2 million of the loss. August loses the most at 3.0 million. Half of all cancellations happen more than 56 days before arrival but 11.9% come within the last week.

## 5. Prepare the data for Power BI

The clean data is saved as `hotel_clean.csv` with 119,203 rows and 42 columns. It is stored in this repository. Power BI connects to it through its GitHub raw link so the dashboard can be refreshed when the file is updated:

`https://raw.githubusercontent.com/ahchu2525/capstone-hosp/main/hotel_clean.csv`
