Answer the following questions and provide the SQL queries used to find the answer.

    
**Question 1: Which cities and countries have the highest level of transaction revenues on the site?**


SQL Queries:
The country-specific query is provided in the listing below.
\begin{lstlisting}[language=SQL, caption=Sample SQL query for ranking countries based on transaction revenue]
with cte_country_city_revenue as
(select v_info.country,v_info.city,se_info.trans_rev 
from (select fullvisitorid, sum(totaltransactionrevenue) as trans_rev
	  from session_info
	  where totaltransactionrevenue is not null
	  group by fullvisitorid)as se_info
join visitor_info as v_info on se_info.fullvisitorid = v_info.fullvisitorid)


select c_rev.country, sum(c_rev.trans_rev) as transactionrevenue,
rank() over(order by sum(c_rev.trans_rev) desc) as rank
from cte_country_city_revenue as c_rev
where c_rev.country is not null
group by c_rev.country
order by rank;
\end{lstlisting}

The city-specific query is provided below.
\begin{lstlisting}[language=SQL, caption=Sample SQL query for inspecting duplicate attributes across tables]
with cte_country_city_revenue as
(select v_info.country,v_info.city,se_info.trans_rev 
from (select fullvisitorid, sum(totaltransactionrevenue) as trans_rev
	  from session_info
	  where totaltransactionrevenue is not null
	  group by fullvisitorid)as se_info
join visitor_info as v_info on se_info.fullvisitorid = v_info.fullvisitorid)

select c_rev.city, sum(c_rev.trans_rev) as transactionrevenue,
rank() over(order by sum(c_rev.trans_rev) desc) as rank
from cte_country_city_revenue as c_rev
where c_rev.city is not null
group by c_rev.city
order by rank;
\end{lstlisting}



Answer:
Countries: United States, Israel, Australia, Canada, and Switzerland
Cities: San Francisco, Sunnyvale, Atlanta, Palo Alto, Tel Aviv-Yafo, New York



**Question 2: What is the average number of products ordered from visitors in each city and country?**


SQL Queries:
The country-specific query is provided in the listing below.
\begin{lstlisting}[language=SQL, caption=Sample SQL query for ranking countries based on transaction revenue]
with cte_country_city_ordered as
(select * 
from (select si.fullvisitorid,vi.country,vi.city,si.productsku
	  from session_info as si
	  join visitor_info as vi using(fullvisitorid))as se_info
join sales_info as s_info on se_info.productsku = s_info.productsku)

select c_rev.country, avg(c_rev.orderedquantity) as avg_ordered,
rank() over(order by avg(c_rev.orderedquantity) desc) as rank
from cte_country_city_ordered as c_rev
where c_rev.country is not null
group by c_rev.country
order by rank;
\end{lstlisting}

The city-specific query is provided below.
\begin{lstlisting}[language=SQL, caption=Sample SQL query for inspecting duplicate attributes across tables]
with cte_country_city_ordered as
(select * 
from (select si.fullvisitorid,vi.country,vi.city,si.productsku
	  from session_info as si
	  join visitor_info as vi using(fullvisitorid))as se_info
join sales_info as s_info on se_info.productsku = s_info.productsku)

select c_rev.city, avg(c_rev.orderedquantity) as avg_ordered,
rank() over(order by avg(c_rev.orderedquantity) desc) as rank
from cte_country_city_ordered as c_rev
where c_rev.city is not null
group by c_rev.city
order by rank;
\end{lstlisting}


Answer:
Excerpts from output
Countries:
Mali 3786
Montenegro 3786
Papua New Guinea 2558
Georgia 2506

Cities:
Council Bluffs   7589
Bellflower       3786
Cork             3786
Santiago         3607
Bellingham       2836




**Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?**


SQL Queries:
The country-specific query is provided in the listing below.
\begin{lstlisting}[language=SQL, caption=Sample SQL query for ranking countries based on transaction revenue]
with cte_country_city_category as
(select * 
from (select si.fullvisitorid,vi.country,vi.city,si.v2productcategory
	  from session_info as si
	  join visitor_info as vi using(fullvisitorid))as se_info
)

select country, v2productcategory, freq
from (select c_rev.country, c_rev.v2productcategory, count(*) as freq,
rank() over(partition by c_rev.country order by count(*) desc) as rank
from cte_country_city_category as c_rev
where c_rev.country is not null
group by c_rev.country,c_rev.v2productcategory
order by freq desc)
where rank = 1;
\end{lstlisting}

The city-specific query is provided below.
\begin{lstlisting}[language=SQL, caption=Sample SQL query for inspecting duplicate attributes across tables]
with cte_country_city_category as
(select * 
from (select si.fullvisitorid,vi.country,vi.city,si.v2productcategory
	  from session_info as si
	  join visitor_info as vi using(fullvisitorid))as se_info
)

select city, v2productcategory, freq
from (select c_rev.city, c_rev.v2productcategory, count(*) as freq,
rank() over(partition by c_rev.city order by count(*) desc) as rank
from cte_country_city_category as c_rev
where c_rev.city is not null
group by c_rev.city,c_rev.v2productcategory
order by freq desc)
where rank = 1;
\end{lstlisting}


Answer:
It is evident from the results that T-shirts for men and Youtube branded merchandise are extremely popular across countries and cities

Excerpts from output
Countries
United States           Men's T-shirts
United Kingdom          YouTube branded merchandise
India                   YouTube branded merchandise

Cities 
Mountain View            Men's T-shirts
New York                 Men's T-shirts
London                    YouTube branded merchandize





**Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?**


SQL Queries:
The country-specific query is provided in the listing below.
\begin{lstlisting}[language=SQL, caption=Sample SQL query for ranking countries based on transaction revenue]
with cte_country_city_category as
(select se_info.country,se_info.city,se_info.productsku,
 s_info.quantitysold,p_info.name
from (select si.fullvisitorid,vi.country,vi.city,si.productsku
	  from session_info as si
	  join visitor_info as vi using(fullvisitorid))as se_info
join sales_info as s_info on se_info.productsku = s_info.productsku
join product_info as p_info on s_info.productsku = p_info.productsku
)


select country, name, freq
from (select c_rev.country, c_rev.name, count(*) as freq,
rank() over(partition by c_rev.country order by count(*) desc) as rank
from cte_country_city_category as c_rev
where c_rev.country is not null
group by c_rev.country,c_rev.name
order by freq desc)
where rank = 1;
\end{lstlisting}

The city-specific query is provided below.
\begin{lstlisting}[language=SQL, caption=Sample SQL query for inspecting duplicate attributes across tables]
with cte_country_city_category as
(select se_info.country,se_info.city,se_info.productsku,
 s_info.quantitysold,p_info.name
from (select si.fullvisitorid,vi.country,vi.city,si.productsku
	  from session_info as si
	  join visitor_info as vi using(fullvisitorid))as se_info
join sales_info as s_info on se_info.productsku = s_info.productsku
join product_info as p_info on s_info.productsku = p_info.productsku
)

select city, name, freq
from (select c_rev.city, c_rev.name, count(*) as freq,
rank() over(partition by c_rev.city order by count(*) desc) as rank
from cte_country_city_category as c_rev
where c_rev.city is not null
group by c_rev.city,c_rev.name
order by freq desc)
where rank = 1;
\end{lstlisting}


Answer:
Excerpt
Countries
United States      Men's 100% Cotton Short Sleeve Hero Tee White
United Kingdom     Twill Cap
India              Custom Decals

Cities
Mountain View      Cam indoor security camera
New York           Twill Cap
San Francisco      Men's 100% Cotton Short Sleeve Hero Tee White





**Question 5: Can we summarize the impact of revenue generated from each city/country?**

How is this question different from Question 1? Check the attached project report file for an analysis on the redundancy in revenue
information between data files. The following is an excerpt of my conclusions.

Information pertaining to the visitor info such as country and city is present in the original all sessions table. As discussed previously, this info is linked to `fullvisitorid` column. Additionally, revenue information is present in both the analytics and all sessions table, but only the 'fullvisitorids` present in the all sessions table has information about the visitor. Finally, as the `transactionrevenue` column is a subset of the `totaltransactionrevenue` column, only the `totaltransactionrevenue` column in the all sessions table is relevant for questions relating to revenue per country/city.



SQL Queries:



Answer:







