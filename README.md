# Ride-Hailing Operations Performance & Cancellation Analysis

## Executive Summary

This project analyses **103,024 ride-booking records** from an Ola/Uber-style ride-hailing dataset for July 2024. The goal is to understand platform reliability, driver and customer cancellations, driver availability issues, vehicle-type performance, and location-level operational risks.

The analysis is designed to match the type of work expected from an **Operations Analyst** in a mobility platform such as DiDi: KPI monitoring, operational insights, dashboarding, cancellation analysis, and data-backed recommendations.

## Business Problem

Ride-hailing platforms rely on fast matching, reliable drivers, and smooth customer experience. In this dataset, only **62.1%** of bookings were successfully completed, meaning **37.9%** of bookings did not convert into completed rides.

The key business question is:

> Why are ride bookings failing, and what operational actions could improve ride completion, driver reliability, and rider experience?

## Dataset Overview

| Item | Description |
|---|---|
| Dataset | Ola & Uber Ride Booking and Cancellation Data |
| Period | July 2024 |
| Rows | 103,024 bookings |
| Key fields | Booking status, vehicle type, pickup/drop location, cancellation reasons, booking value, payment method, ride distance, driver/customer ratings |
| Tools used | Excel, Power Query-style cleaning logic, Pivot Tables, Power BI-ready analysis, Python for chart/export preparation |

## Data Cleaning & Preparation

Main cleaning steps:

1. Removed unusable columns: `Vehicle Images` and `Unnamed: 20`.
2. Converted `Date` and `Time` fields into analysis-ready date/time columns.
3. Created helper fields: `Booking_Hour`, `Day_Name`, `Route`, `Distance_Band`, and `Booking_Value_Band`.
4. Created binary KPI flags: `Is_Success`, `Is_Driver_Cancelled`, `Is_Customer_Cancelled`, and `Is_Driver_Not_Found`.
5. Kept missing values in cancellation fields as meaningful blanks because they depend on booking status.
6. Used successful rides only for completed revenue, distance, payment, and rating analysis.

## KPI Summary

| Metric                           | Value      |
|:---------------------------------|:-----------|
| Total bookings                   | 103,024    |
| Successful rides                 | 63,967     |
| Completion rate                  | 62.1%      |
| Failed booking rate              | 37.9%      |
| Driver cancellation rate         | 17.9%      |
| Customer cancellation rate       | 10.2%      |
| Driver not found rate            | 9.8%       |
| Completed booking value          | 35,080,467 |
| Potential lost booking value     | 21,454,147 |
| Average successful ride distance | 22.9 km    |
| Average driver rating            | 4.0        |
| Average customer rating          | 4.0        |

## Booking Status Breakdown

| Booking_Status       | Bookings   | Share   |
|:---------------------|:-----------|:--------|
| Success              | 63,967     | 62.1%   |
| Canceled by Driver   | 18,434     | 17.9%   |
| Canceled by Customer | 10,499     | 10.2%   |
| Driver Not Found     | 10,124     | 9.8%    |

![Booking Status Breakdown](assets/booking_status_breakdown.png)

### Key insight

The platform completed **63,967** out of **103,024** bookings. Driver-side cancellations account for **17.9%** of all bookings, which is higher than customer-side cancellations at **10.2%**. This suggests driver reliability is a major operational improvement area.

## Hourly Performance

The table below shows the hours with the highest failed booking rates.

|   Booking_Hour | Bookings   | Successful_Rides   | Completion_Rate   | Failed_Rate   |   Driver_Not_Found |
|---------------:|:-----------|:-------------------|:------------------|:--------------|-------------------:|
|             10 | 4,334      | 2,616              | 60.4%             | 39.6%         |                420 |
|             22 | 4,283      | 2,609              | 60.9%             | 39.1%         |                437 |
|              7 | 4,304      | 2,628              | 61.1%             | 38.9%         |                432 |
|             20 | 4,228      | 2,587              | 61.2%             | 38.8%         |                425 |
|             12 | 4,408      | 2,699              | 61.2%             | 38.8%         |                451 |
|              8 | 4,374      | 2,681              | 61.3%             | 38.7%         |                445 |
|              9 | 4,347      | 2,680              | 61.7%             | 38.3%         |                469 |
|             21 | 4,343      | 2,679              | 61.7%             | 38.3%         |                423 |

![Hourly Completion Rate](assets/hourly_completion_rate.png)

### Key insight

Completion rate is relatively stable across the day, but some hours have slightly higher failed booking rates. These time windows should be monitored for driver supply, driver behaviour, and customer cancellation patterns.

## Cancellation Analysis

### Top customer cancellation reasons

| Customer_Cancellation_Reason                 | Bookings   |
|:---------------------------------------------|:-----------|
| Driver is not moving towards pickup location | 3,175      |
| Driver asked to cancel                       | 2,670      |
| Change of plans                              | 2,081      |
| AC is Not working                            | 1,568      |
| Wrong Address                                | 1,005      |

![Top Customer Cancellation Reasons](assets/customer_cancellation_reasons.png)

### Top driver cancellation reasons

| Driver_Cancellation_Reason          | Bookings   |
|:------------------------------------|:-----------|
| Personal & Car related issue        | 6,542      |
| Customer related issue              | 5,413      |
| Customer was coughing/sick          | 3,654      |
| More than permitted people in there | 2,825      |

![Top Driver Cancellation Reasons](assets/driver_cancellation_reasons.png)

### Key insight

The leading customer cancellation reasons include **driver not moving towards pickup location** and **driver asked to cancel**. These are operationally important because they point to driver behaviour after accepting a booking, not just customer preference changes.

Recommended action: monitor driver movement after acceptance, trigger alerts when drivers do not move toward pickup, and review drivers with repeated customer-reported cancellation issues.

## Vehicle Type Performance

| Vehicle_Type   | Bookings   | Successful_Rides   | Completion_Rate   | Driver_Cancellation_Rate   | Customer_Cancellation_Rate   | Driver_Not_Found_Rate   | Completed_Booking_Value   |
|:---------------|:-----------|:-------------------|:------------------|:---------------------------|:-----------------------------|:------------------------|:--------------------------|
| Prime Sedan    | 14,877     | 9,379              | 63.0%             | 17.4%                      | 9.9%                         | 9.7%                    | 5,224,050                 |
| Bike           | 14,662     | 9,134              | 62.3%             | 17.5%                      | 10.5%                        | 9.8%                    | 4,971,968                 |
| Auto           | 14,755     | 9,167              | 62.1%             | 18.1%                      | 10.2%                        | 9.6%                    | 5,051,846                 |
| Mini           | 14,552     | 9,036              | 62.1%             | 17.8%                      | 10.3%                        | 9.8%                    | 4,885,961                 |
| eBike          | 14,816     | 9,180              | 62.0%             | 18.2%                      | 10.2%                        | 9.7%                    | 5,054,662                 |
| Prime Plus     | 14,707     | 9,075              | 61.7%             | 18.2%                      | 10.0%                        | 10.0%                   | 5,015,165                 |
| Prime SUV      | 14,655     | 8,996              | 61.4%             | 18.1%                      | 10.2%                        | 10.3%                   | 4,876,815                 |

![Vehicle Completion Rate](assets/vehicle_completion_rate.png)

### Key insight

Vehicle demand is fairly balanced across vehicle types. Completion rates are also close, but premium categories should still be monitored because small differences in completion rate can represent meaningful lost value at scale.

## Location-Level Operations

### Top pickup locations by demand

| Pickup_Location   | Bookings   | Successful_Rides   | Completion_Rate   |   Driver_Not_Found | Driver_Not_Found_Rate   |
|:------------------|:-----------|:-------------------|:------------------|-------------------:|:------------------------|
| Banashankari      | 2,201      | 1,365              | 62.0%             |                211 | 9.6%                    |
| Yeshwanthpur      | 2,139      | 1,353              | 63.3%             |                204 | 9.5%                    |
| RT Nagar          | 2,135      | 1,321              | 61.9%             |                201 | 9.4%                    |
| Indiranagar       | 2,133      | 1,335              | 62.6%             |                194 | 9.1%                    |
| Sahakar Nagar     | 2,126      | 1,308              | 61.5%             |                218 | 10.3%                   |
| Basavanagudi      | 2,120      | 1,322              | 62.4%             |                212 | 10.0%                   |
| Ramamurthy Nagar  | 2,116      | 1,338              | 63.2%             |                176 | 8.3%                    |
| Vijayanagar       | 2,113      | 1,264              | 59.8%             |                206 | 9.7%                    |
| Tumkur Road       | 2,105      | 1,269              | 60.3%             |                196 | 9.3%                    |
| Cox Town          | 2,100      | 1,335              | 63.6%             |                218 | 10.4%                   |

### Top pickup locations by driver-not-found volume

| Pickup_Location   | Bookings   |   Driver_Not_Found | Driver_Not_Found_Rate   | Completion_Rate   |
|:------------------|:-----------|-------------------:|:------------------------|:------------------|
| Hennur            | 2,090      |                228 | 10.9%                   | 62.5%             |
| Marathahalli      | 2,027      |                225 | 11.1%                   | 61.3%             |
| Peenya            | 2,069      |                220 | 10.6%                   | 61.3%             |
| Mysore Road       | 2,085      |                220 | 10.6%                   | 63.0%             |
| Hosur Road        | 2,074      |                219 | 10.6%                   | 60.2%             |
| Sahakar Nagar     | 2,126      |                218 | 10.3%                   | 61.5%             |
| Cox Town          | 2,100      |                218 | 10.4%                   | 63.6%             |
| Kengeri           | 2,083      |                217 | 10.4%                   | 60.3%             |
| Kammanahalli      | 2,071      |                215 | 10.4%                   | 61.8%             |
| Nagarbhavi        | 2,083      |                215 | 10.3%                   | 62.5%             |

![Top Pickup Locations by Driver Not Found](assets/pickup_driver_not_found.png)

### Key insight

Driver-not-found bookings represent **9.8%** of all bookings. Locations with both high booking volume and high driver-not-found counts should be prioritised for supply-demand balancing, driver repositioning, or targeted driver incentives.

## Route-Level Risk Analysis

The table below highlights high-risk pickup-to-drop routes with at least 30 bookings.

| Route                       |   Bookings |   Successful_Rides |   Failed_Bookings | Completion_Rate   | Failed_Rate   | Driver_Not_Found_Rate   |
|:----------------------------|-----------:|-------------------:|------------------:|:------------------|:--------------|:------------------------|
| Indiranagar → Sarjapur Road |         44 |                 15 |                29 | 34.1%             | 65.9%         | 15.9%                   |
| JP Nagar → BTM Layout       |         37 |                 13 |                24 | 35.1%             | 64.9%         | 16.2%                   |
| Sahakar Nagar → Koramangala |         37 |                 13 |                24 | 35.1%             | 64.9%         | 10.8%                   |
| Banashankari → Majestic     |         50 |                 18 |                32 | 36.0%             | 64.0%         | 16.0%                   |
| Yelahanka → RT Nagar        |         38 |                 14 |                24 | 36.8%             | 63.2%         | 18.4%                   |
| Padmanabhanagar → Peenya    |         34 |                 13 |                21 | 38.2%             | 61.8%         | 17.6%                   |
| Bellandur → Kammanahalli    |         38 |                 15 |                23 | 39.5%             | 60.5%         | 13.2%                   |
| Bellandur → Langford Town   |         43 |                 17 |                26 | 39.5%             | 60.5%         | 16.3%                   |
| Marathahalli → Vijayanagar  |         39 |                 16 |                23 | 41.0%             | 59.0%         | 15.4%                   |
| Langford Town → Chickpet    |         34 |                 14 |                20 | 41.2%             | 58.8%         | 20.6%                   |

### Key insight

Route-level analysis is useful for city operations teams because some routes may be unattractive for drivers, difficult to serve, or affected by local traffic and pickup conditions.

## Revenue and Potential Lost Booking Value

| Booking_Status       | Booking_Value   |
|:---------------------|:----------------|
| Success              | 35,080,467      |
| Canceled by Driver   | 10,183,427      |
| Canceled by Customer | 5,770,901       |
| Driver Not Found     | 5,499,819       |

![Booking Value by Status](assets/booking_value_by_status.png)

### Important note

Booking value from failed rides should be treated as **potential lost booking value**, not actual revenue. Actual completed booking value should only be calculated from successful rides.

## Business Recommendations

1. **Improve driver reliability after booking acceptance**  
   Monitor drivers who repeatedly accept bookings but do not move toward pickup locations.

2. **Target driver supply in high driver-not-found locations**  
   Use pickup-location-level monitoring to identify demand pockets with insufficient supply.

3. **Build a weekly operations KPI dashboard**  
   Track completion rate, driver cancellation rate, customer cancellation rate, driver-not-found rate, booking value, and location-level risk.

4. **Separate customer-side and driver-side cancellation strategies**  
   Customer cancellations and driver cancellations have different causes and should not be treated as one combined problem.

5. **Use route-level insights for city operations planning**  
   Identify routes with high demand but weak completion rates and review whether pricing, pickup design, or driver incentives need improvement.

## Suggested Dashboard Pages

### Page 1: Executive Operations Overview

- Total bookings
- Successful rides
- Completion rate
- Cancellation rate
- Driver-not-found rate
- Completed booking value
- Booking status breakdown
- Daily/hourly booking trend

### Page 2: Cancellation & Reliability Analysis

- Driver vs customer cancellations
- Top cancellation reasons
- Cancellation rate by hour
- Cancellation rate by vehicle type
- High-risk pickup locations

### Page 3: Supply-Demand & City Operations

- Driver-not-found by pickup location
- Completion rate by pickup location
- Vehicle type performance
- Route-level risk table
- Operational recommendations

## Files in This Repository

```text
.
├── README.md
├── Ride_Hailing_Operations_Analysis.xlsx
├── assets/
│   ├── booking_status_breakdown.png
│   ├── hourly_completion_rate.png
│   ├── customer_cancellation_reasons.png
│   ├── driver_cancellation_reasons.png
│   ├── vehicle_completion_rate.png
│   ├── pickup_driver_not_found.png
│   └── booking_value_by_status.png
└── tables/
    ├── kpi_summary.csv
    ├── booking_status_summary.csv
    ├── hourly_performance.csv
    ├── vehicle_type_performance.csv
    ├── pickup_location_performance.csv
    ├── customer_cancellation_reasons.csv
    ├── driver_cancellation_reasons.csv
    └── high_risk_routes.csv
```

## Skills Demonstrated

- Data cleaning and data quality checking
- Excel pivot-style summary tables
- KPI design for operations analysis
- Cancellation reason analysis
- Driver/rider behaviour analysis
- Location and route-level performance analysis
- Business recommendation writing
- Dashboard planning for Power BI or Excel

## Resume Project Description

**Ride-Hailing Operations Performance & Cancellation Analysis | Excel, Pivot Tables, Power BI-ready Dashboard**

- Analysed **103,024 ride-booking records** to monitor platform KPIs including completion rate, cancellation rate, driver-not-found rate, booking value, ride distance, and customer/driver ratings.
- Built pivot-style summary tables and visual dashboards to compare driver and customer cancellations, identify reliability issues, and evaluate vehicle-type and location-level performance.
- Provided data-backed recommendations to improve ride completion, reduce cancellations, and support city-level supply-demand decision-making.
