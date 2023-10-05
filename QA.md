What are your risk areas? Identify and describe them.
The main risk areas were during loading the data, cleaning the data, and normalizing the dataset.

QA after loading data: verify rows and column information by referencing the source data files

QA after cleaning data: verify table information after transformation. Confirm numerical values after scale correction.

QA for normalizing the dataset/understanding the dataset: validate all hypothesis before acting on the table, including those based on uniqueness and functional dependency of attributes. 
  


QA Process:
Describe your QA process and include the SQL queries used to execute it.

Like described above the QA process was categorized into three stages

1) QA after loading data
   It is important to verify that the total rows in the table match the rows in the original CSV files. The below SQL Query provides a sample of how to go about this using the all_sessions table
\begin{lstlisting}[language=SQL, caption=Sample SQL query for verifying rows after data importation]
SELECT COUNT(*)
FROM all_sessions;
\end{lstlisting

2) QA for cleaning the dataset
   It is important to verify that the total rows in the new table match the rows in the original table after the transformations. Additionally, it is good practice to confirm that the numerical values after the scale correction remain consistent. A sample SQL query for the `totaltransactionrevenue` column is provided below.
\begin{lstlisting}[language=SQL, caption=Sample SQL query for verifying rows after data transformation]
SELECT 
SUM(totaltransactionrevenue::numeric) AS total_revenue,
'all_sessions table' AS type
FROM all_sessions

UNION ALL

SELECT 
SUM(totaltransactionrevenue*1000000) AS total_revenue,
'new table' AS type
FROM temp_session_info;
\end{lstlisting}

3) QA for understanding the dataset:
A sample for the all sessions dataset is provided below. See the full report for additional codes.

The following SQL query helps us QA the process of confirming that `currencycode` indeed is unique to the `fullvisitorid`.  An empty row from the below query shows that no one visitor has multiple currencies associated with it from the entire all sessions table.
\begin{lstlisting}[language=SQL, caption=Sample SQL query for verifying dependency of attributes]
select fullvisitorid, count(distinct currencycode) as freq
from all_sessions
group by fullvisitorid
having count(distinct currencycode) > 1
order by freq desc;
\end{lstlisting}



