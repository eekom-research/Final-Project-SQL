What issues will you address by cleaning the data?
The main issues addressed with the data preprocessing and cleaning include:
1) Unavailable data is processed as NULLs
2) The appropriate datatype is selected for each column
3) Scale issues are corrected. E.g., dividing price and revenue data by 1E6


Queries:
Below, provide the SQL queries you used to clean your data.
The cleaning is done per data file and initial table and the code is similar for the different tables. The SQL Query for the all sessions table is provided below. The other tables are cleaned in a similar manner. Check attached Report for more details.

1) For the all sessions dataset
The columns in the all sessions dataset are preprocessed in one go to correct issues such as choosing the correct datatype. For columns such as `country`, `city`, and `productvariant`, rows containing missing data indicated by \textit{(not set)} or \textit{not available in demo dataset} are converted to NULL. This choice is made because the NULL datatype more accurately reflects the states of the rows and the treatment of NULLs in SQL is standardized and feature-rich. Furthermore, columns containing price information such as `totaltransactionrevenue`, `producprice`, `transactionrevenue`, and `productrevenue` had their scales corrected by dividing by $1 \times 10^6$. A new table is created with the preprocessed/cleaned data. The SQL query for this cleaning is provided below.
   
\begin{lstlisting}[language=SQL, caption=Sample SQL query for the all sessions table cleaning]
CREATE TABLE temp_session_info as (
SELECT
	fullvisitorid,
	channelgrouping,
	time::int,
	(CASE
	WHEN country LIKE '(%' OR country LIKE 'not%' THEN NULL
	ELSE country
	END
	) as country,
	(CASE
	WHEN city LIKE '(%' OR city LIKE 'not%' THEN NULL
	ELSE city
	END
	) as city,
	(totaltransactionrevenue::real)/1000000 as totaltransactionrevenue,
	transactions::real,
	timeonsite::int,
	pageviews::int,
	sessionqualitydim::int,
	date::date,
	visitid,
	type,
	productrefundamount,
	productquantity::int,
	(productprice::real)/1000000 as productprice,
	(productrevenue::real)/1000000 as productrevenue,
	productsku,
	v2productname,
	v2productcategory,
	(CASE
	WHEN productvariant LIKE '(%' OR productvariant LIKE 'not%' THEN NULL
	ELSE productvariant
	END
	) as productvariant,
	currencycode,
	itemquantity::int,
	(itemrevenue::real)/1000000 as itemerevenue,
	(transactionrevenue::real)/1000000 as transactionrevenue,
	transactionid,
	pagetitle,
	searchkeyword,
	pagepathlevel1,
	ecommerceactiontype::int,
	ecommerceactionstep::int,
	ecommerceactionoption
FROM all_sessions);
\end{lstlisting}

