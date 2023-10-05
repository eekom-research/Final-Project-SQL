# Final-Project-Transforming-and-Analyzing-Data-with-SQL

## Project/Goals
(fill in your description and goals here)
The primary objective is to solidify recently taught SQL concepts. The goal is to load, clean, understand and generate insights from the given dataset using SQL language.
 

## Process
1) ### Started with understanding the context of the dataset before loading it into the database.
2) ### Next step was to load the data into the database.
3) Then, the data had to be cleaned.
4) After cleaning, it was time to understand the dataset in more detail in order to normalize or work with it. For example, what is the granularity of the data? What does a single row in the particular table represent? This part took a long time during the project. See the report for the detailed steps undertaken.
5) Next, the relationships and ERD between the newly formed and transformed tables is designed/mapped
6) Finally, the dataset is examined for insights into the ecommerce business.
7) During all the stages, QA considerations were need to validate data at each step


## Results
(fill in what you discovered this data could tell you and how you used the data to answer those questions)
After the process of trying to understand the datasets and their inter-relationships, it was quite rewarding to actually answer questions with the data. The whole project exemplified the importance of domain knowledge, understanding your dataset, and fully normalizing your tables. My overall impression is that relational databases and the relational model in general are powerful constructs. Specifically, the following questions where answered. Please see the starting_with_data and starting_with_questions files for more details. Additionally, see the attached report for a fuller picture

Question 1: Which cities and countries have the highest level of transaction revenues on the site?
Details: After separating the visitor information from the visit information, it was easy to answer questions related to transaction revenues. Basically the idea is to use the `fullvisitorid` as a link to get information about the country/cities and the transaction revenues. Then, group by country/city and sum the `totaltransactionrevenue` information. You can then rank or filter afterward.

Question 2: What is the average number of products ordered from visitors in each city and country?
Details: The ordered quantity presumably comprises of quantity sold plus possible refunds. In designing my final tables, I separated all details about sales from product information. Hence, it was easy to solve by linking the information in the newly created sales_info table to get the products ordered and link that back to the session_info and visitor_info tables to get the aggregation by country/city.

Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?
Details: This uses a similar general approach to the above question. Here we are more interested in aggregating across country/city and v2productcategory.

Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold
Details: This uses the product sold information from the sales_info table. The interesting part about this question is ranking across the product after grouping and selecting the number one ranked product from each country/city. 

Question 5: 



## Challenges 
(discuss challenges you faced in the project)
A lot of challenges were encountered, but because the dataset was a mess. I also spent a lot of time creating a report but the submission format does not seem to require it.
1) The products, sales_reports, and sales_by_sku datasets contain duplicated information about products and sales
2) The analytics dataset is a mess. Revenue information is seemingly disaggregated by productsku. But no productsku in the table
3) It was hard to determine the granularity in the all_sessions table.  My conclusion is that I would need more domain information to fully normalize the all_sessions table. For example, what does it mean, precisely, for a single visitid to have more than 1 distinct fullvisitorid?


## Future Goals
(what would you do if you had more time?)
The main issue I would love to address with more time is to fully normalize the tables in the dataset. I made a lot of progress in fixing the product, sales, visitor, and visit related information. See the ERD for details.
