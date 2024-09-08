---
title: "Tableau Dashboard on Covid-19 Cases"
excerpt: "Where were the hotspots during COVID-19 (between Jan 22, 2020 to Apr 29, 2022)?<br/><img src='/images/Tableau.png'>"
collection: projects
date: 2024-06-15
tags:
  - Tableau
  - Cross-Filtering
  - Details on Demand
---


The dashboard can answer the question, "Where were the hotspots during COVID-19 (between Jan 22, 2020, to Apr 29, 2022)?" by analyzing data across various dimensions such as dates, states, and counties. The dashboard can reveal hotspots based on total and new case metrics. 

For example, by filtering the date to January 22, 2020, and selecting the total case metrics while leaving the state and county fields to all, it can be observed that California had the highest total number of positive cases at 1,770,021,359 and the highest total number of deaths at 24,262,380 on that date. By examining the new case metrics for the same date and location, it is evident that California also had the most new positive cases at 6,107,883 and the most new deaths at 59,863. This indicates that on January 22, 2020, the state of California was a hotspot for Covid-19. Similar analysis can be done at the county level, for example, Los Angeles county in California stood out as a hotspot on the same date.

**[Covid-19 Hotspots Dashboard Workbook (Live Preview)](https://public.tableau.com/app/profile/chaoran.zhou/viz/Covid19BookFinalVersion/Dashboard1)**  
_Please change the display layout to the desktop layout for better visualization of the workbook._


### Visual Representation of Data

The two charts indicate the answers through side-by-side proportional symbol maps of cases and side-by-side bar graphs for cases by state/county. 

<img src="/images/Tableau Layout.png" width="450em">

The combination of two charts provides a comprehensive view of the hotspots. The side-by-side proportional symbol maps show geographic areas with different values, allowing users to select multiple states/counties for further investigation. The side-by-side bar graphs then highlight the sorted detail metrics for the selected states/counties, enabling a comparison of positive case counts and death counts.

By comparing values on the map and the bar chart, users can visually identify the hotspots on the specified date.


### Enhancing Dashboard Layout

The layout of the charts promotes visual understanding through consistent color consistency, alignment, and dynamic tooltips. The color consistency, with blue on the left side and purple on the right side for all three charts, allows for easy comparison. The alignment ensures that the left side always represents people's positive case count data, while the right side always represents people's death count data. Additionally, the dynamic tooltips provide detailed information when users hover over specific data points with the same color scheme and alignment. 

The dashboard layout adheres to Ben Schneiderman's mantra of "Overview first. Then, zoom and filter. And then provide details only on demand," enabling users to comprehend the data across multiple charts effectively. The side-by-side bar and line graphs offer an overview of historical data, while the side-by-side map and the side-by-side bar graph for cross-filtering(the second and third chart in the dashboard)facilitate zooming and filtering. All three graphs provide details on demand.


### Details on Demand

The dashboard provides details on demand for all three charts. 

When hovering over the first bar and line chart, the actual data for that day is displayed along with a comparison to the previous day, showing the percentage of increase or decrease. The 7-day smooth line data point also displays the smoothed data point value. Additionally, hovering over the data points also reveals a dynamic bar graph in a tooltip, containing the bar chart for people death count and people positive count. 

For the map in the dashboard, hovering over the data points displays the state and/or county, the data value of the proportional symbol on the map, and a bar graph embedded in the tooltip. This bar graph shows the data value for the user-selected start day and specific state/country.

In the bar chart, when hovering over the data points, it shows the county and states, case type, and case value for that data point.


### Cross-Filtering

The dashboard supports cross-filtering between the second and third charts by using the data from the user-selected start date. This allows users to select on the map based on the proportional symbol and check the highlighting sorted values in the bar chart after selecting multiple values, providing a clearer understanding of their selection.

<img src="/images/Tableau Usage.png" width="450em">

Users can zoom in on the map and further filter by using rectangular selection or by pressing ctrl and selecting multiple values. Hovering over the data points in the map reveals the value and state (and/or county) and the bar chart of both case counts embedded in the tooltip, providing detailed information on demand. The bar graph below the map has sorted bars of both case counts, displaying the grand total of both sides' data on the top.

Both the second and third charts help users to zoom and filter, and if they are interested, they can also get detailed information on demand.


