# Cinema Ticket Sales & Revenue Prediction
This repository contains the project for analyzing cinema ticket sales and customer behavior to predict ticket quantity and sales revenue per transaction and simulate total revenue for a group of customers.

## 1. Project Overview
Accurate forecasting of ticket sales and revenue helps cinema managers plan showtimes, staffing, pricing, and marketing more effectively.
### Using a transactional cinema dataset, this project answers:
How can we predict the future ticket sales and revenue of different movies in a cinema based on historical sales and pricing data?
We focus on transaction-level behavior (customer age, movie genre, seat type, group size, repeat purchase) rather than social media buzz or high-level box office totals.

### Objectives:
- Predict number of tickets per transaction (group size).
- Predict sales revenue per transaction.
- Estimate total revenue for a set of customers using Monte Carlo simulation.
- Identify which factors (seat type, price, group size, genre, age, loyalty) drive revenue the most.

## 2. Dataset
Source: Sarder, H. (2023). Cinema hall ticket sales and customer behavior (Kaggle).
Observations: 1,440 transactions
Variables (raw):
1. TicketID – unique transaction ID
2. Age – customer age
3. TicketPrice – price per ticket
4. MovieGenre – Action, Comedy, Drama, Horror, Sci-Fi
5. SeatType – Standard, Premium, VIP
6. NumberofPerson – number of people in the transaction (group size; includes “Alone”)
7. PurchaseAgain – Yes/No repeat purchase indicator

Engineered variables:
1. NumberofPerson cleaned ("Alone" → 1, converted to integer).
2. PurchaseAgain encoded (Yes → 1, No → 0).
3. SalesRevenue = TicketPrice * NumberofPerson to measure revenue per transaction.

## 3. Methods
Data preparation
1. Loaded the original Excel/CSV file into pandas.
2. Verified no missing values across variables.
3. Cleaned NumberofPerson and encoded PurchaseAgain.
4. Dropped TicketID (non‑predictive ID variable).
5. Saved a cleaned dataset for modeling.

Modeling and analysis
Train–test split: 70% training, 30% validation.
Regression models:
- Linear regression for ticket price (to see if pricing varies by customer/genre/seat).
- Linear regression for number of persons per transaction (group size).
- Linear regression for sales revenue per transaction using:
  + TicketPrice, NumberofPerson, MovieGenre, SeatType, Age, PurchaseAgain.
Logistic regression:
  + Predict PurchaseAgain (probability of repeat purchase) from age, price, and genre/seat preferences.
Evaluation:
  + Used RMSE, MSE, and R2 on the validation set.
  + Tested different classification thresholds for the logistic model.
Monte Carlo simulation:
  + 1,000 trials to estimate total revenue for 500 customers by sampling historical TicketPrice and NumberofPerson.

All code is implemented in the Jupyter notebook in this repository.

## 4. Key Findings
- Ticket price is effectively fixed by the cinema’s pricing policy and not meaningfully explained by age, genre, seat type, or repeat purchase behavior.
- Group size (NumberofPerson) is the strongest driver of SalesRevenue; more people per transaction directly yield higher revenue.
- VIP seats tend to be associated with slightly larger groups and higher revenue per transaction than Standard or Premium seats.
- Movie genre and age have little impact on ticket quantity compared to seat type and group size.
- The revenue model achieves a high R2, indicating that ticket price, group size, and seat type together explain most variation in transaction revenue.
- The PurchaseAgain logistic model performs close to random, suggesting that current variables are not sufficient to reliably predict repeat visits.
These insights support strategies that emphasize group bookings and premium/VIP seat promotion, rather than focusing heavily on genre or demographic‑based pricing.

## 5. How to Run the Notebook
Requirements:
- Python 3.x
- Typical data science libraries:
  + pandas
  + numpy
  + scikit-learn
  + statsmodels
  + matplotlib / seaborn

## 6. Usage and Extensions
This project can be adapted or extended by:
- Adding time‑based features (day of week, showtime, holiday flags) to improve forecasting.
- Including promotion or marketing variables to measure campaign impact on attendance and revenue.
- Trying tree‑based models (Random Forest, Gradient Boosting) to capture non‑linear relationships.
- Expanding the Monte Carlo simulation to estimate revenue and occupancy across multiple screens and showtimes.

## 7. Limitations
- The dataset does not include showtime, date, or holiday information, which limits the ability to capture time‑based demand patterns.
- Repeat purchase behavior (PurchaseAgain) is hard to model with the available variables, leading to low predictive power for loyalty.
- The analysis focuses on per‑transaction revenue, not full cinema‑wide scheduling or capacity constraints.
