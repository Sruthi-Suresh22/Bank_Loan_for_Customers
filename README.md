# BANK LOAN FOR CUSTOMERS

### Project Description

This project helps to analyze the loan amount given by a financial institution to different customers of varied grades and sub-grade levels. This project takes into consideration the loan disbursement reasons, funded amount, and revolving balance values for every customer in different states and geolocation. It also takes into account the customer payment modes and last payment value.

### Dataset Details

- Domain : Finance
- Project : Bank Loan of Customers
- Datasets : There are 2 Datasets available - **Finance_1.csv** & **Finance_2.xlsx**
- Dataset Type : Excel Data
- Dataset Size : Each excel file has 39717 records

### KPI's

1. Year wise loan amount Stats
2. Grade and sub grade wise revol_bal
3. Total Payment for Verified Status Vs Non Verified Status
4. State wise and last_credit_pull_d wise loan status
5. Home ownership Vs last payment date stats
6. Top 5 Loan Purpose by Sum of Loan Amount
7. Average Interest Rate
8. Total Funded Amount
9. Average Funded Amount
10. Average Debt-to-income Ratio
11. Total Loan Amount
12. Total Number of Loans Issued

### SQL Analysis

- **Year wise loan amount Stats**
  
*Query:*
```sql
SELECT year(issue_d) AS "Year", SUM(loan_amnt) AS "Loan Amount" 
FROM finance_1 
GROUP BY Year 
ORDER BY Year DESC;
```
<details>
<summary><i>Output:</i></summary>  
  
| Year |  Loan Amount |
|------|--------------|
| 2011 | 260506575    |
| 2010 | 122050200    |
| 2009 | 46436325     |
| 2008 | 14390275     |
| 2007 | 2219275      |
</details>

- **Grade and sub grade wise revol_bal**

*Query:*
```sql
SELECT finance_1.grade AS "Grade", finance_1.sub_grade AS "Sub-Grade", SUM(finance_2.revol_bal) AS "Revol_Bal" 
FROM finance_1 INNER JOIN finance_2 ON finance_1.id = finance_2.id 
GROUP BY finance_1.grade, finance_1.sub_grade
ORDER BY finance_1.grade; 
```
<details>
<summary><i>Output:</i></summary> 

| Grade |  Sub-Grade |  Revol_Bal |
|-------|------------|------------|
| A     | A1         | 11365196   |
| A     | A2         | 14004780   |
| A     | A3         | 19543922   |
| A     | A4         | 34557156   |
| A     | A5         | 35303045   |
| B     | B1         | 21842079   |
| B     | B2         | 26478439   |
| B     | B3         | 39723554   |
| B     | B4         | 35405811   |
| B     | B5         | 37858666   |
| C     | C1         | 29384926   |
| C     | C2         | 27321114   |
| C     | C3         | 20531370   |
| C     | C4         | 16867691   |
| C     | C5         | 16015609   |
| D     | D1         | 12130255   |
| D     | D2         | 18570972   |
| D     | D3         | 16793781   |
| D     | D4         | 13742947   |
| D     | D5         | 13252474   |
| E     | E1         | 11132588   |
| E     | E2         | 10242033   |
| E     | E3         | 9039059    |
| E     | E4         | 7990991    |
| E     | E5         | 7669868    |
| F     | F1         | 5840746    |
| F     | F2         | 4528248    |
| F     | F3         | 3175435    |
| F     | F4         | 2551064    |
| F     | F5         | 2187323    |
| G     | G1         | 1808763    |
| G     | G2         | 1729627    |
| G     | G3         | 832193     |
| G     | G4         | 1390628    |
| G     | G5         | 701515     |
</details>

- **Total Payment for Verified Status Vs Non Verified Status**

*Query:*
```sql
SELECT finance_1.verification_status AS "Verification Status", ROUND(SUM(finance_2.total_pymnt),2) AS "Total Payment"
FROM finance_1 INNER JOIN finance_2 ON finance_1.id = finance_2.id
WHERE NOT finance_1.verification_status = "Source Verified"
GROUP BY finance_1.verification_status;
```
<details>
<summary><i>Output:</i></summary>

| Verification Status |  Total Payment |
|---------------------|----------------|
| Not Verified        | 153541418.21   |
| Verified            | 219892307.51   |
</details>

- **State wise and last_credit_pull_d wise loan status**

*Query:*
```sql
SELECT YEAR(finance_2.last_credit_pull_d) AS "Year(Last credit pull date)",MONTHNAME(finance_2.last_credit_pull_d) AS "Month(Last credit pull date)", finance_1.addr_state AS "State", finance_1.loan_status AS "Loan Status" ,COUNT(finance_1.loan_status) AS "Count of Loan Status"
FROM finance_1 INNER JOIN finance_2 ON finance_1.id = finance_2.id
GROUP BY finance_1.addr_state, finance_1.loan_status, YEAR(finance_2.last_credit_pull_d),MONTHNAME(finance_2.last_credit_pull_d)
ORDER BY YEAR(finance_2.last_credit_pull_d),MONTHNAME(finance_2.last_credit_pull_d);
```

<details>
<summary><i>Output:</i></summary>
  
>[!NOTE]

>There are 1000+ Records for this query. The records corresponding to the year 2007 are shown below:

| Year(Last credit pull date) |  Month(Last credit pull date) |  State |  Loan Status |  Count of Loan Status |
|-----------------------------|-------------------------------|--------|--------------|-----------------------|
| 2007                        | August                        | WA     | Fully Paid   | 2                     |
| 2007                        | August                        | WI     | Fully Paid   | 1                     |
| 2007                        | August                        | GA     | Fully Paid   | 1                     |
| 2007                        | August                        | MA     | Fully Paid   | 1                     |
| 2007                        | August                        | CA     | Fully Paid   | 9                     |
| 2007                        | August                        | NY     | Fully Paid   | 1                     |
| 2007                        | December                      | TX     | Fully Paid   | 1                     |
| 2007                        | December                      | LA     | Fully Paid   | 1                     |
| 2007                        | July                          | FL     | Fully Paid   | 1                     |
| 2007                        | June                          | GA     | Fully Paid   | 1                     |
| 2007                        | June                          | MD     | Fully Paid   | 1                     |
| 2007                        | June                          | MA     | Fully Paid   | 4                     |
| 2007                        | June                          | IN     | Fully Paid   | 1                     |
| 2007                        | June                          | WI     | Fully Paid   | 5                     |
| 2007                        | May                           | NY     | Fully Paid   | 1                     |
| 2007                        | October                       | NC     | Fully Paid   | 1                     |
| 2007                        | October                       | SC     | Fully Paid   | 1                     |
| 2007                        | October                       | UT     | Fully Paid   | 1                     |
| 2007                        | September                     | IN     | Fully Paid   | 1                     |
| 2007                        | September                     | NC     | Fully Paid   | 1                     |
</details>

- **Home ownership Vs last payment date stats**

*Query:*
```sql
SELECT YEAR(finance_2.last_pymnt_d) AS "Year (Last payment date)",MONTHNAME(finance_2.last_pymnt_d) AS "Month (Last payment date)",finance_1.home_ownership AS "Home Ownership",COUNT(finance_1.home_ownership) AS "Count of Home Ownership"
FROM finance_1 INNER JOIN finance_2 ON finance_1.id = finance_2.id
GROUP BY  YEAR(finance_2.last_pymnt_d),MONTHNAME(finance_2.last_pymnt_d),finance_1.home_ownership
ORDER BY YEAR(finance_2.last_pymnt_d),MONTHNAME(finance_2.last_pymnt_d);
```
<details>
<summary><i>Output:</i></summary>

| Year (Last payment date) |  Month (Last payment date) |  Home Ownership |  Count of Home Ownership |
|--------------------------|----------------------------|-----------------|--------------------------|
| -                        | -                          | MORTGAGE        | 14                       |
| -                        | -                          | OWN             | 5                        |
| -                        | -                          | RENT            | 52                       |
| 2008                     | April                      | MORTGAGE        | 5                        |
| 2008                     | April                      | OWN             | 3                        |
| 2008                     | April                      | RENT            | 4                        |
| 2008                     | August                     | MORTGAGE        | 3                        |
| 2008                     | August                     | OWN             | 1                        |
| 2008                     | August                     | RENT            | 9                        |
| 2008                     | December                   | MORTGAGE        | 7                        |
| 2008                     | December                   | OWN             | 1                        |
| 2008                     | December                   | RENT            | 6                        |
| 2008                     | February                   | RENT            | 1                        |
| 2008                     | January                    | MORTGAGE        | 3                        |
| 2008                     | January                    | OWN             | 1                        |
| 2008                     | July                       | MORTGAGE        | 7                        |
| 2008                     | July                       | RENT            | 7                        |
| 2008                     | June                       | MORTGAGE        | 4                        |
| 2008                     | June                       | OWN             | 1                        |
| 2008                     | June                       | RENT            | 5                        |
| 2008                     | March                      | MORTGAGE        | 2                        |
| 2008                     | March                      | OWN             | 1                        |
| 2008                     | March                      | RENT            | 2                        |
| 2008                     | May                        | MORTGAGE        | 9                        |
| 2008                     | May                        | OWN             | 1                        |
| 2008                     | May                        | RENT            | 4                        |
| 2008                     | November                   | MORTGAGE        | 4                        |
| 2008                     | November                   | OWN             | 1                        |
| 2008                     | November                   | RENT            | 5                        |
| 2008                     | October                    | MORTGAGE        | 10                       |
| 2008                     | October                    | OWN             | 1                        |
| 2008                     | October                    | RENT            | 17                       |
| 2008                     | September                  | MORTGAGE        | 2                        |
| 2008                     | September                  | RENT            | 10                       |
| 2009                     | April                      | MORTGAGE        | 18                       |
| 2009                     | April                      | OWN             | 4                        |
| 2009                     | April                      | RENT            | 21                       |
| 2009                     | August                     | MORTGAGE        | 26                       |
| 2009                     | August                     | OWN             | 2                        |
| 2009                     | August                     | RENT            | 21                       |
| 2009                     | December                   | MORTGAGE        | 36                       |
| 2009                     | December                   | OTHER           | 1                        |
| 2009                     | December                   | OWN             | 4                        |
| 2009                     | December                   | RENT            | 52                       |
| 2009                     | February                   | MORTGAGE        | 12                       |
| 2009                     | February                   | OTHER           | 2                        |
| 2009                     | February                   | OWN             | 3                        |
| 2009                     | February                   | RENT            | 16                       |
| 2009                     | January                    | MORTGAGE        | 11                       |
| 2009                     | January                    | OTHER           | 1                        |
| 2009                     | January                    | OWN             | 1                        |
| 2009                     | January                    | RENT            | 8                        |
| 2009                     | July                       | MORTGAGE        | 19                       |
| 2009                     | July                       | OTHER           | 2                        |
| 2009                     | July                       | OWN             | 5                        |
| 2009                     | July                       | RENT            | 17                       |
| 2009                     | June                       | MORTGAGE        | 12                       |
| 2009                     | June                       | OTHER           | 2                        |
| 2009                     | June                       | OWN             | 1                        |
| 2009                     | June                       | RENT            | 25                       |
| 2009                     | March                      | MORTGAGE        | 16                       |
| 2009                     | March                      | OTHER           | 1                        |
| 2009                     | March                      | OWN             | 3                        |
| 2009                     | March                      | RENT            | 18                       |
| 2009                     | May                        | MORTGAGE        | 15                       |
| 2009                     | May                        | OWN             | 4                        |
| 2009                     | May                        | RENT            | 22                       |
| 2009                     | November                   | MORTGAGE        | 27                       |
| 2009                     | November                   | OTHER           | 1                        |
| 2009                     | November                   | OWN             | 4                        |
| 2009                     | November                   | RENT            | 27                       |
| 2009                     | October                    | MORTGAGE        | 22                       |
| 2009                     | October                    | OTHER           | 1                        |
| 2009                     | October                    | OWN             | 7                        |
| 2009                     | October                    | RENT            | 32                       |
| 2009                     | September                  | MORTGAGE        | 15                       |
| 2009                     | September                  | OTHER           | 1                        |
| 2009                     | September                  | OWN             | 1                        |
| 2009                     | September                  | RENT            | 20                       |
| 2010                     | April                      | MORTGAGE        | 54                       |
| 2010                     | April                      | OWN             | 6                        |
| 2010                     | April                      | RENT            | 54                       |
| 2010                     | August                     | MORTGAGE        | 53                       |
| 2010                     | August                     | OTHER           | 1                        |
| 2010                     | August                     | OWN             | 18                       |
| 2010                     | August                     | RENT            | 79                       |
| 2010                     | December                   | MORTGAGE        | 95                       |
| 2010                     | December                   | OWN             | 17                       |
| 2010                     | December                   | RENT            | 141                      |
| 2010                     | February                   | MORTGAGE        | 34                       |
| 2010                     | February                   | OTHER           | 1                        |
| 2010                     | February                   | OWN             | 35                       |
| 2010                     | February                   | RENT            | 42                       |
| 2010                     | January                    | MORTGAGE        | 18                       |
| 2010                     | January                    | OWN             | 6                        |
| 2010                     | January                    | RENT            | 49                       |
| 2010                     | July                       | MORTGAGE        | 81                       |
| 2010                     | July                       | OTHER           | 1                        |
| 2010                     | July                       | OWN             | 14                       |
| 2010                     | July                       | RENT            | 92                       |
| 2010                     | June                       | MORTGAGE        | 40                       |
| 2010                     | June                       | OTHER           | 2                        |
| 2010                     | June                       | OWN             | 8                        |
| 2010                     | June                       | RENT            | 66                       |
| 2010                     | March                      | MORTGAGE        | 55                       |
| 2010                     | March                      | OTHER           | 2                        |
| 2010                     | March                      | OWN             | 9                        |
| 2010                     | March                      | RENT            | 72                       |
| 2010                     | May                        | MORTGAGE        | 58                       |
| 2010                     | May                        | OTHER           | 2                        |
| 2010                     | May                        | OWN             | 12                       |
| 2010                     | May                        | RENT            | 44                       |
| 2010                     | November                   | MORTGAGE        | 65                       |
| 2010                     | November                   | OTHER           | 3                        |
| 2010                     | November                   | OWN             | 20                       |
| 2010                     | November                   | RENT            | 107                      |
| 2010                     | October                    | MORTGAGE        | 76                       |
| 2010                     | October                    | OTHER           | 3                        |
| 2010                     | October                    | OWN             | 23                       |
| 2010                     | October                    | RENT            | 114                      |
| 2010                     | September                  | MORTGAGE        | 77                       |
| 2010                     | September                  | NONE            | 2                        |
| 2010                     | September                  | OTHER           | 1                        |
| 2010                     | September                  | OWN             | 17                       |
| 2010                     | September                  | RENT            | 79                       |
| 2011                     | April                      | MORTGAGE        | 174                      |
| 2011                     | April                      | OTHER           | 6                        |
| 2011                     | April                      | OWN             | 37                       |
| 2011                     | April                      | RENT            | 203                      |
| 2011                     | August                     | MORTGAGE        | 197                      |
| 2011                     | August                     | OTHER           | 2                        |
| 2011                     | August                     | OWN             | 31                       |
| 2011                     | August                     | RENT            | 198                      |
| 2011                     | December                   | MORTGAGE        | 248                      |
| 2011                     | December                   | OTHER           | 4                        |
| 2011                     | December                   | OWN             | 44                       |
| 2011                     | December                   | RENT            | 248                      |
| 2011                     | February                   | MORTGAGE        | 136                      |
| 2011                     | February                   | NONE            | 1                        |
| 2011                     | February                   | OTHER           | 3                        |
| 2011                     | February                   | OWN             | 28                       |
| 2011                     | February                   | RENT            | 169                      |
| 2011                     | January                    | MORTGAGE        | 92                       |
| 2011                     | January                    | OWN             | 26                       |
| 2011                     | January                    | RENT            | 148                      |
| 2011                     | July                       | MORTGAGE        | 172                      |
| 2011                     | July                       | OTHER           | 2                        |
| 2011                     | July                       | OWN             | 32                       |
| 2011                     | July                       | RENT            | 188                      |
| 2011                     | June                       | MORTGAGE        | 155                      |
| 2011                     | June                       | OTHER           | 2                        |
| 2011                     | June                       | OWN             | 30                       |
| 2011                     | June                       | RENT            | 189                      |
| 2011                     | March                      | MORTGAGE        | 179                      |
| 2011                     | March                      | OTHER           | 7                        |
| 2011                     | March                      | OWN             | 43                       |
| 2011                     | March                      | RENT            | 259                      |
| 2011                     | May                        | MORTGAGE        | 177                      |
| 2011                     | May                        | OTHER           | 3                        |
| 2011                     | May                        | OWN             | 32                       |
| 2011                     | May                        | RENT            | 174                      |
| 2011                     | November                   | MORTGAGE        | 190                      |
| 2011                     | November                   | OTHER           | 3                        |
| 2011                     | November                   | OWN             | 40                       |
| 2011                     | November                   | RENT            | 221                      |
| 2011                     | October                    | MORTGAGE        | 207                      |
| 2011                     | October                    | OWN             | 48                       |
| 2011                     | October                    | RENT            | 198                      |
| 2011                     | September                  | MORTGAGE        | 204                      |
| 2011                     | September                  | OTHER           | 2                        |
| 2011                     | September                  | OWN             | 31                       |
| 2011                     | September                  | RENT            | 213                      |
| 2012                     | April                      | MORTGAGE        | 287                      |
| 2012                     | April                      | OTHER           | 3                        |
| 2012                     | April                      | OWN             | 61                       |
| 2012                     | April                      | RENT            | 383                      |
| 2012                     | August                     | MORTGAGE        | 361                      |
| 2012                     | August                     | OTHER           | 4                        |
| 2012                     | August                     | OWN             | 67                       |
| 2012                     | August                     | RENT            | 400                      |
| 2012                     | December                   | MORTGAGE        | 289                      |
| 2012                     | December                   | OTHER           | 1                        |
| 2012                     | December                   | OWN             | 71                       |
| 2012                     | December                   | RENT            | 346                      |
| 2012                     | February                   | MORTGAGE        | 322                      |
| 2012                     | February                   | OTHER           | 4                        |
| 2012                     | February                   | OWN             | 59                       |
| 2012                     | February                   | RENT            | 350                      |
| 2012                     | January                    | MORTGAGE        | 231                      |
| 2012                     | January                    | OWN             | 46                       |
| 2012                     | January                    | RENT            | 269                      |
| 2012                     | July                       | MORTGAGE        | 306                      |
| 2012                     | July                       | OTHER           | 3                        |
| 2012                     | July                       | OWN             | 61                       |
| 2012                     | July                       | RENT            | 371                      |
| 2012                     | June                       | MORTGAGE        | 299                      |
| 2012                     | June                       | OTHER           | 2                        |
| 2012                     | June                       | OWN             | 55                       |
| 2012                     | June                       | RENT            | 346                      |
| 2012                     | March                      | MORTGAGE        | 362                      |
| 2012                     | March                      | OTHER           | 4                        |
| 2012                     | March                      | OWN             | 63                       |
| 2012                     | March                      | RENT            | 415                      |
| 2012                     | May                        | MORTGAGE        | 306                      |
| 2012                     | May                        | OTHER           | 6                        |
| 2012                     | May                        | OWN             | 56                       |
| 2012                     | May                        | RENT            | 368                      |
| 2012                     | November                   | MORTGAGE        | 302                      |
| 2012                     | November                   | OTHER           | 1                        |
| 2012                     | November                   | OWN             | 71                       |
| 2012                     | November                   | RENT            | 366                      |
| 2012                     | October                    | MORTGAGE        | 361                      |
| 2012                     | October                    | OTHER           | 3                        |
| 2012                     | October                    | OWN             | 54                       |
| 2012                     | October                    | RENT            | 408                      |
| 2012                     | September                  | MORTGAGE        | 360                      |
| 2012                     | September                  | OTHER           | 4                        |
| 2012                     | September                  | OWN             | 36                       |
| 2012                     | September                  | RENT            | 361                      |
| 2013                     | April                      | MORTGAGE        | 399                      |
| 2013                     | April                      | OWN             | 56                       |
| 2013                     | April                      | RENT            | 396                      |
| 2013                     | August                     | MORTGAGE        | 319                      |
| 2013                     | August                     | OWN             | 63                       |
| 2013                     | August                     | RENT            | 345                      |
| 2013                     | December                   | MORTGAGE        | 346                      |
| 2013                     | December                   | OWN             | 60                       |
| 2013                     | December                   | RENT            | 374                      |
| 2013                     | February                   | MORTGAGE        | 389                      |
| 2013                     | February                   | OWN             | 74                       |
| 2013                     | February                   | RENT            | 406                      |
| 2013                     | January                    | MORTGAGE        | 360                      |
| 2013                     | January                    | OWN             | 60                       |
| 2013                     | January                    | RENT            | 364                      |
| 2013                     | July                       | MORTGAGE        | 345                      |
| 2013                     | July                       | OWN             | 70                       |
| 2013                     | July                       | RENT            | 361                      |
| 2013                     | June                       | MORTGAGE        | 311                      |
| 2013                     | June                       | OWN             | 43                       |
| 2013                     | June                       | RENT            | 337                      |
| 2013                     | March                      | MORTGAGE        | 452                      |
| 2013                     | March                      | OWN             | 71                       |
| 2013                     | March                      | RENT            | 503                      |
| 2013                     | May                        | MORTGAGE        | 421                      |
| 2013                     | May                        | OWN             | 58                       |
| 2013                     | May                        | RENT            | 428                      |
| 2013                     | November                   | MORTGAGE        | 303                      |
| 2013                     | November                   | OWN             | 50                       |
| 2013                     | November                   | RENT            | 318                      |
| 2013                     | October                    | MORTGAGE        | 304                      |
| 2013                     | October                    | OWN             | 47                       |
| 2013                     | October                    | RENT            | 341                      |
| 2013                     | September                  | MORTGAGE        | 305                      |
| 2013                     | September                  | OWN             | 51                       |
| 2013                     | September                  | RENT            | 328                      |
| 2014                     | April                      | MORTGAGE        | 297                      |
| 2014                     | April                      | OWN             | 44                       |
| 2014                     | April                      | RENT            | 333                      |
| 2014                     | August                     | MORTGAGE        | 392                      |
| 2014                     | August                     | OWN             | 54                       |
| 2014                     | August                     | RENT            | 386                      |
| 2014                     | December                   | MORTGAGE        | 353                      |
| 2014                     | December                   | OWN             | 68                       |
| 2014                     | December                   | RENT            | 524                      |
| 2014                     | February                   | MORTGAGE        | 347                      |
| 2014                     | February                   | OWN             | 52                       |
| 2014                     | February                   | RENT            | 393                      |
| 2014                     | January                    | MORTGAGE        | 382                      |
| 2014                     | January                    | OWN             | 53                       |
| 2014                     | January                    | RENT            | 397                      |
| 2014                     | July                       | MORTGAGE        | 385                      |
| 2014                     | July                       | OWN             | 51                       |
| 2014                     | July                       | RENT            | 384                      |
| 2014                     | June                       | MORTGAGE        | 351                      |
| 2014                     | June                       | OWN             | 70                       |
| 2014                     | June                       | RENT            | 357                      |
| 2014                     | March                      | MORTGAGE        | 358                      |
| 2014                     | March                      | OWN             | 56                       |
| 2014                     | March                      | RENT            | 410                      |
| 2014                     | May                        | MORTGAGE        | 304                      |
| 2014                     | May                        | OWN             | 46                       |
| 2014                     | May                        | RENT            | 332                      |
| 2014                     | November                   | MORTGAGE        | 304                      |
| 2014                     | November                   | OWN             | 45                       |
| 2014                     | November                   | RENT            | 240                      |
| 2014                     | October                    | MORTGAGE        | 383                      |
| 2014                     | October                    | OWN             | 78                       |
| 2014                     | October                    | RENT            | 347                      |
| 2014                     | September                  | MORTGAGE        | 316                      |
| 2014                     | September                  | OWN             | 56                       |
| 2014                     | September                  | RENT            | 321                      |
| 2015                     | April                      | MORTGAGE        | 64                       |
| 2015                     | April                      | OWN             | 6                        |
| 2015                     | April                      | RENT            | 66                       |
| 2015                     | August                     | MORTGAGE        | 106                      |
| 2015                     | August                     | OWN             | 9                        |
| 2015                     | August                     | RENT            | 95                       |
| 2015                     | December                   | MORTGAGE        | 80                       |
| 2015                     | December                   | OWN             | 14                       |
| 2015                     | December                   | RENT            | 82                       |
| 2015                     | February                   | MORTGAGE        | 85                       |
| 2015                     | February                   | OWN             | 9                        |
| 2015                     | February                   | RENT            | 69                       |
| 2015                     | January                    | MORTGAGE        | 107                      |
| 2015                     | January                    | OWN             | 34                       |
| 2015                     | January                    | RENT            | 191                      |
| 2015                     | July                       | MORTGAGE        | 146                      |
| 2015                     | July                       | OTHER           | 1                        |
| 2015                     | July                       | OWN             | 14                       |
| 2015                     | July                       | RENT            | 89                       |
| 2015                     | June                       | MORTGAGE        | 120                      |
| 2015                     | June                       | OWN             | 13                       |
| 2015                     | June                       | RENT            | 87                       |
| 2015                     | March                      | MORTGAGE        | 99                       |
| 2015                     | March                      | OWN             | 13                       |
| 2015                     | March                      | RENT            | 70                       |
| 2015                     | May                        | MORTGAGE        | 78                       |
| 2015                     | May                        | OWN             | 8                        |
| 2015                     | May                        | RENT            | 61                       |
| 2015                     | November                   | MORTGAGE        | 118                      |
| 2015                     | November                   | OWN             | 17                       |
| 2015                     | November                   | RENT            | 92                       |
| 2015                     | October                    | MORTGAGE        | 97                       |
| 2015                     | October                    | OWN             | 10                       |
| 2015                     | October                    | RENT            | 79                       |
| 2015                     | September                  | MORTGAGE        | 107                      |
| 2015                     | September                  | OWN             | 13                       |
| 2015                     | September                  | RENT            | 82                       |
| 2016                     | April                      | MORTGAGE        | 116                      |
| 2016                     | April                      | OWN             | 15                       |
| 2016                     | April                      | RENT            | 89                       |
| 2016                     | February                   | MORTGAGE        | 79                       |
| 2016                     | February                   | OWN             | 17                       |
| 2016                     | February                   | RENT            | 73                       |
| 2016                     | January                    | MORTGAGE        | 91                       |
| 2016                     | January                    | OWN             | 17                       |
| 2016                     | January                    | RENT            | 75                       |
| 2016                     | March                      | MORTGAGE        | 113                      |
| 2016                     | March                      | OWN             | 17                       |
| 2016                     | March                      | RENT            | 86                       |
| 2016                     | May                        | MORTGAGE        | 705                      |
| 2016                     | May                        | OWN             | 94                       |
| 2016                     | May                        | RENT            | 457                      |  
</details>

- **Top 5 Loan Purpose by Sum of Loan Amount**

*Query:*
```sql
SELECT purpose AS "Purpose", SUM(loan_amnt) AS "Loan Amount" 
FROM finance_1 
GROUP BY purpose 
ORDER BY SUM(loan_amnt) DESC LIMIT 5;
```
<details>
<summary><i>Output:</i></summary>

| Purpose            |  Loan Amount |
|--------------------|--------------|
| debt_consolidation | 236647300    |
| credit_card        | 60142150     |
| home_improvement   | 34334725     |
| other              | 32213975     |
| small_business     | 24800975     |  
</details>

- **Average Interest Rate**

*Query:*
```sql
SELECT AVG(int_rate) AS "Average Interest Rate" FROM finance_1;
```
<details>
<summary><i>Output:</i></summary>

| Average Interest Rate |
|-----------------------|
| 12.021176574262988    |
</details>

- **Total Funded Amount**

*Query:*
```sql
SELECT SUM(funded_amnt) AS "Total Funded Amount" FROM finance_1;
```
<details>
<summary><i>Output:</i></summary>

| Total Funded Amount |
|---------------------|
| 434810325           |
</details>

- **Average Funded Amount**

*Query:*
```sql
SELECT AVG(funded_amnt) AS "Average Funded Amount" FROM finance_1;
```
<details>
<summary><i>Output:</i></summary>

| Average Funded Amount |
|-----------------------|
| 10947.7132            |
</details>

- **Average Debt-to-income Ratio**

*Query:*
```sql
SELECT AVG(dti) AS "Debt-to-income Ratio" FROM finance_1;
```
<details>
<summary><i>Output:</i></summary>

| Debt-to-income Ratio |
|----------------------|
| 13.315129541883262   |
</details>

- **Total Loan Amount**

*Query:*
```sql
SELECT SUM(loan_amnt) AS "Total Loan Amount" FROM finance_1;
```
<details>
<summary><i>Output:</i></summary>

| Total Loan Amount |
|-------------------|
| 445602650         |
</details>

- **Total Number of Loans Issued**

*Query:*
```sql
SELECT COUNT(id) AS "Total Number of Loans Issued" FROM finance_1;
```
<details>
<summary><i>Output:</i></summary>

| Total Number of Loans Issued |
|------------------------------|
| 39717                        |
</details>

### EXCEL DASHBOARD
![Bank Loan for Analysis](https://github.com/Sruthi-Suresh22/Bank_Loan_for_Customers_SQL/assets/162356465/a69748e9-1899-4933-b160-05f27e85f936)








