# **Plant Co. Gross Profit Performance Dashboard**  

## **Overview**  
This Power BI project delivers an interactive and comprehensive dashboard for analyzing **Plant Co.'s Gross Profit Performance**. The dashboard incorporates key metrics and visualizations to provide actionable insights into Year-to-Date (YTD) performance, Previous Year-to-Date (PYTD) comparisons, and profitability segmentation across regions, months, and product categories.

---

## **Data Model**  

### **1. Date Dimension Table**  
A **Dim_Date** table was created using DAX to generate a continuous date range from **January 1, 2022**, to **December 31, 2024**.  

```DAX
Dim_Date = 
    CALENDAR(
        DATE(2022, 1, 1), 
        DATE(2024, 12, 31)
    )
```

### **2. Identifying Past Data for Comparative Analysis**  
The `Inpast` column flags dates within the past relative to the last recorded sales date for time-based analysis.  

```DAX
Inpast = 
VAR lastsalesdate = MAX(Fact_Sales[Date_Time])
VAR lastsalesdatePY = EDATE(lastsalesdate, -12)
RETURN
Dim_Date[Date] <= lastsalesdatePY
```

---

## **Key Performance Measures**  

### **1. Year-to-Date (YTD) and Previous Year-to-Date (PYTD) Metrics**  
- **PYTD_Sales**: Captures sales for the same period in the previous year.  
```DAX
PYTD_Sales = 
CALCULATE(
    [Sales], 
    SAMEPERIODLASTYEAR(Dim_Date[Date]),
    FILTER(Dim_Date, Dim_Date[Inpast] = TRUE)
)
```

- **YTD_Sales**: Aggregates sales for the current year.  
```DAX
YTD_Sales = TOTALYTD([Sales], Fact_Sales[Date_Time])
```

- **Dynamic Switching Measures**: Enable toggling between KPIs (Sales, Quantity, Gross Profit).  
```DAX
S_YTD = 
VAR selected_value = SELECTEDVALUE(Slc_Values[Values])
VAR result = SWITCH(selected_value,
    "Sales", [YTD_Sales],
    "Quantity", [YTD_Quantity],
    "Gross Profit", [YTD_GrossProfit],
    BLANK()
)
RETURN result
```

### **2. Comparative Analysis: YTD vs. PYTD**  
A measure to compare YTD and PYTD values.  
```DAX
YTD_vs_PYTD = [S_YTD] - [S_PYTD]
```

---

## **Dashboard Features**  

### **Visual Components**
![Gross Profit Performance Dashboard](path/to/image.png)  

1. **KPI Cards**:  
   - YTD Gross Profit: **$1.40M**  
   - YTD vs. PYTD Difference: **-$5.49M**  
   - PYTD Gross Profit: **$6.89M**  
   - GP%: **39.77%**

2. **Treemap**: Highlights the **bottom 10 countries** by YTD vs. PYTD performance.  
   - **China** shows the largest negative contribution: **-1.93M**.  

3. **Waterfall Chart**: Month-wise impact of YTD vs. PYTD Gross Profit across countries and products.  

4. **Line and Clustered Column Chart**:  
   - Tracks **YTD and PYTD Gross Profit trends** by month and product type (**Indoor, Landscape, Outdoor**).  

5. **Scatter Plot**: Displays account profitability segmentation by **GP% and YTD Values**.  

### **Dynamic Features**
- **Slicers**: Enable filtering by time, product type, and region.  
- **Dynamic Titles**: Visual headers update based on user-selected filters for contextual relevance.

---

## **Key Insights**  
This dashboard provides actionable insights into Plant Co.'s sales performance:  
- Highlights underperforming regions and products.  
- Tracks trends in profitability and sales over time.  
- Identifies accounts with high or low profitability for targeted strategies.  

---

## **How to Use**  
1. Import the **Power BI file** into your Power BI Desktop.  
2. Connect it to your dataset.  
3. Explore the dashboard using the slicers and dynamic features for detailed analysis.  

---

This README serves as a detailed guide to your project. Let me know if you’d like to refine or add further details!
