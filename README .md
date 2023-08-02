# Data Analysis

## Part 1: SQL

Some fraudsters hack a credit card by making several small transactions (generally less than $2.00), which are typically ignored by cardholders.

  - How can you isolate (or group) the transactions of each cardholder? 

    ```sql
    -- Part1 : Grouping transactions of each card returning card numbers only
    SELECT card
    FROM transaction 
    GROUP BY card;

    -- Part1 : Grouping by card and joining cardholder name, cardholder id and their total transaction amounts
    SELECT card_holder.name, credit_card.cardholder_id, transaction.card, sum(amount) as total_spent
    FROM transaction 
    LEFT JOIN credit_card
    on transaction.card = credit_card.card
    LEFT JOIN card_holder
    on credit_card.cardholder_id = card_holder.id
    GROUP BY transaction.card, credit_card.cardholder_id, card_holder.name;
    ```
    - **Output:** [Group cardholder transactions](./output/grouping_cardholders.csv)

  - Count the transactions that are less than $2.00 per cardholder. 

    ```sql
    -- Part1 : Count transactions less than $2 per cardholder
    SELECT card, COUNT(amount) as tx_less_than_2
    FROM transaction
    WHERE amount < 2
    GROUP BY card
    ORDER BY tx_less_than_2 DESC;
    ```
    
   - Is there any evidence to suggest that a credit card has been hacked? Explain your rationale.
      - **Answer:** 
        - *Yes, Possily. With the assumption of the small transaction hack, cards with the highest count of transations less than $2 have possibly been hacked.*

    - [x] Take your investigation a step futher by considering the time period in which potentially fraudulent transactions are made. 

      ```sql
      -- Part1 : Count transactions less than $2 per cardholder group by date
        SELECT date, card, COUNT(amount) as tx_less_than_2
        FROM transaction
        WHERE amount < 2
        GROUP BY card, date
        ORDER BY tx_less_than_2 DESC;
       ```


  - What are the top 100 highest transactions made between 7:00 am and 9:00 am? 

    ```sql
    -- Part1 : Top 100 highest transactions made between 7:00am and 9:00am
    SELECT date, amount
    FROM transaction
    WHERE EXTRACT(HOUR FROM date) BETWEEN '07' AND '08'
    ORDER BY amount DESC
    FETCH FIRST 100 ROWS ONLY;
    ```


  - Do you see any anomalous transactions that could be fraudulent
    - **Answer:**
      - *No*
  - Is there a higher number of fraudulent transactions made during this time frame versus the rest of the day?
    - **Answer:**
      - *No, the data does not show any correlation to higher number of fradulent transactions.*
  - If you answered yes to the previous question, explain why you think there might be fraudulent transactions during this time frame.

  - What are the top 5 merchants prone to being hacked using small transactions? 

    ```sql
    -- Part1 : Top 5 merchants prone to being hacked using small transactions
    SELECT id_merchant, count(amount) as number_of_small_tx
    FROM transaction
    WHERE amount < 2
    GROUP BY id_merchant
    ORDER BY count(amount) DESC
    LIMIT 5;
    ```


## Part 2: Visual Analysis

The two most important customers of the firm may have been hacked. Verify if there are any fraudulent transactions in their history. For privacy reasons, you only know that their cardholder IDs are 2 and 18.

  - [x] Using hvPlot, create a line plot representing the time series of transactions over the course of the year for each cardholder separately. 

    - Cardholder 2
      ![Cardholder 2 plot](./cardholder_2_line_plot.png)  

    - Cardholder 18
      ![Cardholder 18 plot](./images/cardholder_18_line_plot.png) 

  - [x] Next, to better compare their patterns, create a single line plot that contains both card holders' trend data.  

    ![Combined cardholder plots](./combined_carholder_2%2618_plots.png)

  - What difference do you observe between the consumption patterns? Does the difference suggest a fraudulent transaction? Explain your rationale.
    - **Answer:**
      * *Cardholder 2 has a small dollar amount spending habit while Cardholder 18 has a larger dollar amount spending habit. The difference in spending amounts can indicate fradulent activity on the card if the spending amount is significantly different from their normal spending. For Example, Cardholder 18 has transactions of less than $2, different from their usual larger dollar amount spendidng habit. This could suggest fradulent activity, however this data alone is inconclusive.*

  - [x] Using Plotly Express, create a box plot, representing the expenditure data from January 2018 to June 2018 for cardholder ID 25.

    ![Cardholder 25 January to June bar plot](./cardholder_25_bar_jan_to_june.png)

  - [x] **Question:** Are there any outliers for cardholder ID 25? How many outliers are there per month?
    - **Answer:**
      - *Yes. There are multiple putliers at least once a month*
  - [x] **Question:** Do you notice any anomalies? Describe your observations and conclusions.
    - **Answer:**
      - *Yes, there is anomalies. Cardholder 25 has extremely large dollar amount transactions and extremely low dollar amount transactions.*
 the following articles:

