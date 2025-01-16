# INF6027 Project

This repository hosts the **INF6027 Project**, which focuses on retail sales analysis in R. It contains scripts for data cleaning, exploratory data analysis (EDA), correlation, and linear regression modelling.

---

## Table of Contents
1. [Overview](#overview)
2. [System Requirements](#system-requirements)
3. [Installation and Setup](#installation-and-setup)
4. [Running the Scripts](#running-the-scripts)
5. [Project Structure](#project-structure)
6. [License](#license)

---

## Overview
- **Course**: INF6027
- **Goal**: A study to analyse the retail sales performance in UK.
- **Aims and Objectives**: The study findings aimed to analyse the monthly, weekly and annual retail sales trends in the United Kingdom (UK) from pre-COVID (2019) to post-COVID (2020), focusing around on non-store retailing and overall retail sales. This study even seeks to identify relevant correlations, predict retail sales growth, and uncover patterns in retail industry trends, including and excluding automotive fuel.
- **Methods**:
  - **Exploratory Data Analysis (EDA)**
  - **Correlation Analysis**
  - **Linear Regression** for predictive modeling

---

## System Requirements
- **Operating System**: Windows, macOS, or Linux
- **R Version**: 4.1.2 or later
- **R Packages**:
  - `dplyr`
  - `ggplot2`
  - `readr`
  - `tidyr`
  - Others (as needed, specified in your scripts)

---

## Installation and Setup
1. **Clone the Repository**:
   - Click the green "**Code**" button in this GitHub repository.
   - Copy the HTTPS or SSH link provided (e.g., `https://github.com/rabraham2/INF6027-Project.git`).
   - Open your terminal or command prompt and run:
     ```bash
     git clone https://github.com/rabraham2/INF6027-Project.git
     ```

2. **Install Required R Packages**:
   Open **R** or **RStudio** and install the necessary packages:

```r
   install.packages("readxl")
   install.packages("dplyr")
   install.packages("tidyr")
   install.packages("tidyverse")
   install.packages("scales")
   install.packages("forcats")
   install.packages("ggplot2")
   install.packages("gridExtra")
   install.packages("corrplot")

```

## Running the Scripts

```r

# Start of Introduction of Data Science Coursework

install.packages("readxl")
install.packages("dplyr")
install.packages("tidyr")
install.packages("tidyverse")
install.packages("scales")
install.packages("forcats")
install.packages("ggplot2")
install.packages("gridExtra")
install.packages("corrplot")

library(readxl)
library(dplyr)
library(tidyr)
library(tidyverse)
library(scales)
library(forcats)
library(ggplot2)
library(gridExtra)
library(corrplot)

# Reading the Retail Pounds Dataset used for Visualization

retail_sales_pounds_data <- read_excel("Retail_Sales_Index_Pounds_Data.xlsx")
view(retail_sales_pounds_data)
str(retail_sales_pounds_data)


# Visualization of Monthly Sales using Clustered Bar Chart 2020 vs 2019

# Data Filtering based on Month

retail_sales_pound_data_month_on_month <- retail_sales_pounds_data %>% filter(valnsat_time_period_year >= 2019 & valnsat_time_period_year <= 2020) %>%
  select(
    valnsat_time_period_year,
    valnsat_time_period_month,
    valnsat_no_of_weeks_per_month,
    valnsat_all_retailing_including_automotive_fuel,
    valnsat_non_store_retailing_category_3,
    valnsat_value_of_avg_monthly_weekly_retail_sales_including_automobile_fuel,
    valnsat_month_as_a_percentage_of_total
  )

# Converting the 'valnsat_time_period_month' to a numerals months in order

valnsat_month_in_numerals <- c(
  "JAN" = "01",
  "FEB" = "02",
  "MAR" = "03",
  "APR" = "04",
  "MAY" = "05",
  "JUN" = "06",
  "JUL" = "07",
  "AUG" = "08",
  "SEP" = "09",
  "OCT" = "10",
  "NOV" = "11",
  "DEC" = "12"
)

# Converting the month abbreviations in 'valnsat_time_period_month' to corresponding month numbers

retail_sales_pound_data_month_on_month$valnsat_time_period_month_num <- valnsat_month_in_numerals[retail_sales_pound_data_month_on_month$valnsat_time_period_month]

# Plotting the clustered bar chart with monthly sales values of 2020 and 2019

ggplot(retail_sales_pound_data_month_on_month, aes(x = valnsat_time_period_month_num, 
                                             y = valnsat_all_retailing_including_automotive_fuel, 
                                             fill = factor(valnsat_time_period_year, levels = c(2020, 2019)))) +
  geom_bar(stat = "identity", position = "dodge", width = 0.7) +
  scale_fill_manual(values = c("2020" = "darkblue", "2019" = "lightblue")) +
  geom_line(aes(
    group = factor(valnsat_time_period_year, levels = c(2020, 2019)), 
    color = factor(valnsat_time_period_year, levels = c(2020, 2019))
  ), 
  size = 1, 
  linetype = "solid") +
  labs(
    title = "Month-on-Month Sales for 2020 vs 2019",
    x = "Month on Month (in Months)",
    y = "Monthly Sales including Automotive Fuel (in £thousands)",
    fill = "Time Periods",
    color = "Monthly Sales Trend"
  ) +
  scale_x_discrete(
    breaks = c("01", "02", "03", "04", "05", "06", "07", "08", "09", "10", "11", "12"),
    labels = month.abb
  ) +
  scale_y_continuous(labels = label_number(scale = 1e-6, suffix = "M")) +
  theme(
    axis.text.x = element_text(angle = 45, hjust = 1),
    plot.title = element_text(hjust = 0.5))


# Visualization of Monthly Non-Store Retailing Sales using Clustered Bar Chart 2020 vs 2019

# Data Filtering based on Month

non_store_retailing_sales_pound_data_month_on_month <- retail_sales_pounds_data %>% filter(valnsat_time_period_year >= 2019 & valnsat_time_period_year <= 2020) %>%
  select(
    valnsat_time_period_year,
    valnsat_time_period_month,
    valnsat_no_of_weeks_per_month,
    valnsat_non_store_retailing_category_3,
    valnsat_value_of_avg_monthly_weekly_retail_sales_including_automobile_fuel,
    valnsat_month_as_a_percentage_of_total
  )

# Converting the month abbreviations in 'valnsat_time_period_month' to corresponding month numbers

non_store_retailing_sales_pound_data_month_on_month$valnsat_time_period_month_num <- valnsat_month_in_numerals[non_store_retailing_sales_pound_data_month_on_month$valnsat_time_period_month]

# Plotting the clustered bar chart with monthly non-store retailing sales values of 2020 and 2019

ggplot(non_store_retailing_sales_pound_data_month_on_month, aes(x = valnsat_time_period_month_num, 
                                                   y = valnsat_non_store_retailing_category_3, 
                                                   fill = factor(valnsat_time_period_year, levels = c(2020, 2019)))) +
  geom_bar(stat = "identity", position = "dodge", width = 0.7) +
  scale_fill_manual(values = c("2020" = "darkblue", "2019" = "lightblue")) +
  geom_line(aes(
    group = factor(valnsat_time_period_year, levels = c(2020, 2019)), 
    color = factor(valnsat_time_period_year, levels = c(2020, 2019))
  ), 
  size = 1, 
  linetype = "solid") +
  labs(
    title = "Month-on-Month Non-store Retailing Sales for 2020 vs 2019",
    x = "Month on Month (in Months)",
    y = "Monthly Sales Non-store Retailing (in £thousands)",
    fill = "Time Periods",
    color = "Monthly Non-store Retailing Sales Trend"
  ) +
  scale_x_discrete(
    breaks = c("01", "02", "03", "04", "05", "06", "07", "08", "09", "10", "11", "12"),
    labels = month.abb
  ) +
  scale_y_continuous(labels = label_number(scale = 1e-6, suffix = "M")) +
  theme(
    axis.text.x = element_text(angle = 45, hjust = 1),
    plot.title = element_text(hjust = 0.5))


# Filter data for 2019 and 2020 only
retail_sales_pounds_data_2019_and_2020 <- retail_sales_pounds_data %>%
  filter(valnsat_time_period_year %in% c(2019, 2020))

# Verify the filtered dataset
str(retail_sales_pounds_data_2019_and_2020)


# Investigate Relationships (Research Question 1: Relationship Between Total Sales and Non-Store Retailing)

# Remove rows with NA in relevant columns
correlation_retail_sales_data <- retail_sales_pounds_data_2019_and_2020 %>%
  filter(!is.na(valnsat_all_retailing_including_automotive_fuel) &
           !is.na(valnsat_non_store_retailing_category_3))

# Calculate correlation
correlation_2019_and_2020 <- cor(
  correlation_retail_sales_data$valnsat_all_retailing_including_automotive_fuel,
  correlation_retail_sales_data$valnsat_non_store_retailing_category_3,
  method = "pearson"
)

# Display the correlation result
print(paste("Correlation between retail sales and non-store retailing: ", correlation_2019_and_2020))


# Linear Regression Model (Research Question 2: Predicting Retail Sales Growth)

# Filter data for 2019 and 2020
linear_regression_data_2019_and_2020<- retail_sales_pounds_data %>%
  filter(valnsat_time_period_year %in% c(2019, 2020)) %>%
  select(
    valnsat_all_retailing_including_automotive_fuel,
    valnsat_non_store_retailing_category_3,
    valnsat_automotive_fuel
  ) %>%
  drop_na()

# Fit the linear regression model
linear_regression_model <- lm(
  valnsat_all_retailing_including_automotive_fuel ~ 
    valnsat_non_store_retailing_category_3 + 
    valnsat_automotive_fuel, 
  data = linear_regression_data_2019_and_2020
)

# Summary of the regression model
summary(linear_regression_model)

# Printing R-squared value
r_squared <- summary(linear_regression_model)$r.squared
cat("R-squared value of the model:", r_squared, "\n")

# Comparative Analysis (Research Question 3: Patterns in Sales Trends (Including vs Excluding Automotive Fuel))

# Filter relevant automobile fuel sales data for 2019 and 2020
comparative_analysis_retail_sales_data <- retail_sales_pounds_data %>%
  filter(valnsat_time_period_year %in% c(2019, 2020)) %>%
  select(
    valnsat_time_period_year,
    valnsat_time_period_month,
    valnsat_all_retailing_including_automotive_fuel,
    valnsat_all_retailing_excluding_automotive_fuel
  )

comparative_analysis_retail_sales_data$valnsat_time_period_month_num <- valnsat_month_in_numerals[comparative_analysis_retail_sales_data$valnsat_time_period_month]


# Summarize retail sales data including and excluding automobile fuel sales by year and month
comparative_analysis_sales_summary <- comparative_analysis_retail_sales_data %>%
  group_by(valnsat_time_period_year, valnsat_time_period_month_num) %>%
  summarise(
    including_fuel = sum(valnsat_all_retailing_including_automotive_fuel, na.rm = TRUE),
    excluding_fuel = sum(valnsat_all_retailing_excluding_automotive_fuel, na.rm = TRUE)
  )

# Create a long format for visualization
comparative_analysis_sales_categorisation <- comparative_analysis_sales_summary %>%
  pivot_longer(
    cols = c(including_fuel, excluding_fuel),
    names_to = "Category",
    values_to = "Sales"
  )

# Plot the line chart for comparative analysis
ggplot(comparative_analysis_sales_categorisation, aes(
  x = as.numeric(valnsat_time_period_month_num),
  y = Sales / 1e6,  # Convert sales to millions
  color = Category,
  linetype = as.factor(valnsat_time_period_year)
)) +
  geom_line(size = 1) +
  labs(
    title = "Retail Sales Trends (Including vs Excluding Automotive Fuel Sales)",
    x = "Month (in Numerical Order)",
    y = "Sales (in £Millions)",
    color = "Sales Type",
    linetype = "Year"
  ) +
  scale_x_continuous(
    breaks = 1:12,
    labels = month.abb
  ) +
  theme_minimal() +
  theme(
    plot.title = element_text(hjust = 0.5),
    axis.text.x = element_text(angle = 45, hjust = 1)
  )

```

## Project Structure

INF6027-Project/
├─ Code/
│  ├─ data_r_code.R
├─ Data/
│  ├─ Retail_Sales_Index_Pounds_Data.xlsx
├─ Results/
│  ├─ Month-on-Month-Retail-Sales-2020-vs-2019.png
│  ├─ Month-on-Month-Non-store-Retailing-Sales-2020-vs-2019.png
│  ├─ Retail-Sales-Trends-Including-vs-Excluding-Automotive-Fuel-Sales.png
├─ LICENSE
├─ README.md

## License

MIT License

Copyright (c) 2025 rabraham2

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
