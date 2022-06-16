# Exercise 4

## Context

Your manager has a couple of reports they want you to run. They would like you to:

Assuming the order statuses are:
| id  | status_name              |
|:---:|--------------------------|
|  1  | On Hold                  |
|  2  | Paid but not delivered   |
|  3  | Paid and shipped         |
|  4  | Recieved by the cusomter |

- List the amount of customers in New York and the total revenue per store as long as the store revenue was higher than $2,000.00
- List the total number of children, electric and mountain bikes that are on hold in the New York store
- The average number of orders per month
- The amount of bikes that have been recieved by the customer from the Texas store and sold by Bernardine or Layla

You might need:
- [IN](https://www.postgresqltutorial.com/postgresql-tutorial/postgresql-in/)
- [DATE_TRUNC](https://www.postgresqltutorial.com/postgresql-date-functions/postgresql-date_trunc/)
