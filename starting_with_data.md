Question 1: What products are among the top 10 sold but whose stock level are currently below the average stock level for all products?

SQL Queries:
select productsku, name, stocklevel
from project.product_info
where productsku in (select productsku
	   				from project.sales_info
	   				order by quantitysold desc
	   				limit 10) 
and stocklevel < (select avg(stocklevel)
				 from project.product_info)
order by stocklevel

Answer: " Doodle Decal"
" Women's Long Sleeve Blended Cardigan Charcoal"
"Android Youth Short Sleeve T-shirt Pewter"
"Android Women's Short Sleeve Badge Tee Dark Heather"
" Women's Quilted Insulated Vest Black"
" Toddler Short Sleeve Tee White"
" Men's Watershed Full Zip Hoodie Grey"
"Android Youth Short Sleeve T-shirt Pewter"



Question 2: The top 4 most important channel groups based on transactions revenue

SQL Queries: 
select channelgrouping, sum(totaltransactionrevenue) as trans_rev,
rank() over(order by sum(totaltransactionrevenue) desc)
from project.session_info
where totaltransactionrevenue is not null
group by channelgrouping

Answer:
"Referral"	            6018.680000000001	    1
"Direct"	              4704.190032	2
"Organic Search"	      3163.6699679999997	3
"Paid Search"	          394.77	4


Question 3: What is the transaction revenue from each country per year?

SQL Queries: 
select extract(year from date) as year,vi.country, sum(totaltransactionrevenue)
from project.session_info as ss
join project.visitor_info as vi using(fullvisitorid)
where totaltransactionrevenue is not null
group by extract(year from date), vi.country
order by year desc

Answer: 
2017	      "Australia"	          358
2017	      "Canada"	            82.16
2017	      "Israel"	            602
2017	      "Switzerland"	        16.99
2017	      "United States"	    8790.489967999996
2016	      "Canada"	          67.99
2016	      "United States"	    4363.680032




Question 4: What are the top 10 products with average sentiment score greater than the average sentiment of all products?

SQL Queries:
select productsku,name, avg(sentimentscore) as sentiment
from project.product_info
group by productsku
having avg(sentimentscore) > (select avg(sentimentscore)
							 from project.product_info)
order by sentiment desc
limit 10;
	   

Answer:
"GGOBJGOWUSG69402"	"USB wired soundbar - in store only"	1
"GGOEGEVB071799"	" Pocket Bluetooth Speaker"	0.8999999761581421
"9182741"	" Men's Airflow 1/4 Zip Pullover Lapis"	0.8999999761581421
"GGOEGDHC018299"	" 22 oz Water Bottle"	0.8999999761581421
"GGOEAAEJ030913"	"Android Women's Long Sleeve Blended Cardigan Grey"	0.8999999761581421
"GGOEGAAB058913"	" Men's Short Sleeve Performance Badge Tee Black"	0.8999999761581421
"GGOEGOCL077699"	" Hard Cover Journal"	0.8999999761581421
"GGOEGAAX0682"	" Youth Short Sleeve Tee Red"	0.8999999761581421


Question 5: For each product, what is the most important channel based on site visits?

SQL Queries:
select *
from (select pi.name,si.channelgrouping, count(*) as freq,
rank() over(partition by pi.name order by count(*) desc) as rank
from project.product_info as pi
join project.session_info as si using(productsku)
group by pi.name,si.channelgrouping
order by freq desc)
where rank = 1


Answer:
" Twill Cap"	"Organic Search"	226	1
"22 oz  Bottle Infuser"	"Organic Search"	196	1
" Men's 100% Cotton Short Sleeve Hero Tee White"	"Organic Search"	181	1
" Custom Decals"	"Organic Search"	166	1
" Trucker Hat"	"Organic Search"	165	1
" Leatherette Notebook Combo"	"Organic Search"	150	1
