# Hotel-Dashboard
Regional Hotel Intelligence Dashboard Notes
# Hotel Revenue Intelligence Dashboard – Le Meridien Focus

## 📌 Project Overview

This Power BI project explores regional hotel performance and customer behavior using a dataset sourced from Kaggle. The primary focus is Le Meridien in Sabah, with comparative insights from two competing hotels in Malaysia. The goal is to uncover forecasting and segmentation insights to support actionable business strategies.

---

## 🪑 Data Preparation

1. **Source**: Dataset downloaded from Kaggle (Hotel Sales)
2. **Data Cleaning**: Performed in Excel (null handling, column renaming, format correction)
3. **Data Import**: Cleaned dataset loaded into Power BI as a single table: `Hotels Data Clean`
4. **Data Modeling**:

   * Created calculated tables to support time intelligence and forecasting
   * Built a custom date table and marked it for accurate sorting by month/year

---

## 📂 Dataset Overview

**Main Table**: `Hotels Data Clean`

**Key Columns Used**:

* **Hotel & Customer Info**: `hotel_name`, `hotel_type`, `region`, `state`, `customer_name`, `customer_segment`, `room_type`, `booking_channel`
* **Financials**: `price`, `nights`, `discount`, `disc_amt`, `sales`, `avg_daily_rate`
* **Dates**: `arrival_date`, `arrival_month`, `departure_month`

---

## 📊 Objective 1: Forecast Monthly Revenue

**Goal**: Predict monthly room revenue trends for Le Meridien to support staff planning and sales strategy.

### Power BI Steps:

**1. Created a Date Table**

```DAX
DateTable = CALENDAR(MIN('Hotels Data Clean'[arrival_date]), MAX('Hotels Data Clean'[departure_date]))
```

**2. Supporting Columns**

```DAX
Year = YEAR('DateTable'[Date])
Month = FORMAT('DateTable'[Date], "MMMM")
MonthNumber = MONTH('DateTable'[Date])
YearMonth = FORMAT('DateTable'[Date], "YYYY-MM")
```

* Marked as a Date Table
* Created a relationship with `'Hotels Data Clean'[arrival_date]`

**3. Total Sales Measure**

```DAX
Total Sales = SUM('Hotels Data Clean'[sales])
```

**4. Line Chart Visualization**

* X-axis: `DateTable[YearMonth]`
* Y-axis: `Total Sales`
* Forecast Length: 12 months (using Analytics pane)

**Insight**: The Studio Room leads with \~\$520,948 in forecasted sales, while the Royal Suite trails at \~\$126,579.

---

## 📈 Objective 2: Predict Staff Requirements Based on Booking Patterns

**Goal**: Estimate staff needs based on monthly booking trends to support efficient scheduling.

### Power BI Steps:

**1. Relationship:**

* Linked `'DateTable'[Date]` to `'Hotels Data Clean'[arrival_date]`

**2. Total Bookings Measure**

```DAX
Total Bookings = COUNTROWS('Hotels Data Clean')
```

**3. Estimated Staff Measure**

```DAX
Estimated Staff Required = ROUNDUP([Total Bookings] / 5, 0)
```

**4. Area Chart Visualization**

* X-axis: `DateTable[YearMonth]`
* Y-axis: `Estimated Staff Required`
* Slicers: `room_type`, `hotel_name`
* Trend Line added

**Insight**: Studio room bookings peak in January, March, June, and September, requiring \~43 staff during high-demand periods.

---

## 🔍 Objective 3: Segment Analysis for Targeted Marketing

**Goal**: Analyze customer segment contributions to sales by room type to enable targeted marketing.

### Power BI Steps:

**1. Total Sales Measure**

```DAX
Total Sales = SUM('Hotels Data Clean'[sales])
```

**2. Stacked Column Chart**

* X-axis: `customer_segment`
* Y-axis: `Total Sales`
* Legend: `room_type`
* Slicers: `booking_channel`, `region`, `room_type`

**Insight**: Corporate segment leads with over \$6.5M in total sales, but no bookings were recorded for the Royal Suite. Opportunity exists to promote luxury options to this high-value group.

---

## 📊 Objective 4: Booking Channel Performance

**Goal**: Identify the most profitable booking channel and evaluate the cost of discounts offered.

### Power BI Steps:

**1. Created Core Measures**

```DAX
Total Nights = SUM('Hotels Data Clean'[nights])
Total Discount = SUM('Hotels Data Clean'[disc_amt])
Total Sales = SUM('Hotels Data Clean'[sales])
```

**2. Visualizations**

* **Clustered Bar Chart**

  * Y-axis: `MonthLabel`
  * X-axis: `Total Nights`
  * Legend: `booking_channel`
* **Stacked Area Chart**

  * X-axis: `booking_channel`
  * Y-axis: `Total Discount`

**3. Additional Elements**

* Slicers for `hotel_name`, `room_type`
* Table: `customer_segment` vs. `Total Nights`
* Card: Total Nights Booked

**Insight**: Booking.com drives the highest room-night revenue (~~\$54K) but also requires the most discount spending (~~\$54K). Direct bookings are the most cost-efficient, with the lowest nights and discount spend (\~\$48K and \~\$45K respectively).

---

**End of README**
