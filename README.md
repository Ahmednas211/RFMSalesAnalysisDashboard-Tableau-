# RFMSalesAnalysisDashboard-Tableau
## Project Objective
The RFM sales analysis study was done using SQL, and this project gives an interactive and simplified view of it. </br>
## Tableau Visualization  </br>
<img src="https://raw.githubusercontent.com/Ahmednas211/RFMSalesAnalysisDashboard-Tableau-/main/dashboard.jpeg"/>  </br>
## SQL Queries </br>
### -- INSPECT DATA </br>
#### -- check imported dataset                                                 </br>
SELECT * </br>
FROM SalesDataSample..sales_data_sample </br>

#### -- total count of unique orders </br>
SELECT COUNT(DISTINCT ORDERNUMBER) </br>
FROM SalesDataSample..sales_data_sample </br>
#### -- THERE ARE 307 UNIQUE ORDERS </br>
#### -- total count of orders </br>
SELECT COUNT(ORDERNUMBER) </br>
FROM SalesDataSample..sales_data_sample </br>
#### -- THERE ARE 2823 ORDERS </br>
#### -- total sum of quantity order </br>
SELECT SUM(QUANTITYORDERED) </br>
FROM SalesDataSample..sales_data_sample </br>
#### -- THERE ARE 99,067 QUANTITIES ORDERED </br>
#### -- NOTE 1: SALES = PRICEEACH * QUANTITYORDERED </br>
#### -- NOTE 2: PRICEEACH IS NOT A CONSTANT VARIABLE WRT ORDERLINENUMBER </br>
#### -- total count of order line </br>
SELECT COUNT(DISTINCT ORDERLINENUMBER) </br>
FROM SalesDataSample..sales_data_sample </br>
#### -- THERE ARE 18 ORDER LINE </br>
#### -- total sum of sales </br>
SELECT SUM(SALES) </br>
FROM SalesDataSample..sales_data_sample </br>
#### -- THE TOTAL SUM OF SALES IS 10,032,628.85 </br>
#### -- total count of unique status </br>
SELECT COUNT(DISTINCT STATUS) </br>
FROM SalesDataSample..sales_data_sample </br>
#### -- THERE ARE 6 UNIQUE STATUS </br>
#### -- date range </br>
SELECT DISTINCT YEAR_ID </br>
FROM SalesDataSample..sales_data_sample </br>
ORDER BY YEAR_ID </br>
#### -- THIS DATASET RANGES BETWEEN 2003 TO 2005 
#### -- check if each year has a complete month 
SELECT COUNT(DISTINCT MONTH_ID) </br>
FROM SalesDataSample..sales_data_sample </br>
WHERE YEAR_ID = 2003 </br>
#### -- 2003 HAS 12 COMPLETE MONTHS 

SELECT COUNT(DISTINCT MONTH_ID) </br>
FROM SalesDataSample..sales_data_sample </br>
WHERE YEAR_ID = 2004 </br>
#### -- 2004 HAS 12 CALENDER MONTHS 

SELECT COUNT(DISTINCT MONTH_ID)                  </br>
FROM SalesDataSample..sales_data_sample                 </br>
WHERE YEAR_ID = 2005                 </br>
#### -- 2005 HAS 5 CALENDER MONTHS
SELECT DISTINCT MONTH_ID                 </br>
FROM SalesDataSample..sales_data_sample                 </br>
WHERE YEAR_ID = 2005                 </br>
ORDER BY MONTH_ID                 </br>

#### -- VIZ 1, 2, 3, 4, 5
#### -- NOTE 3: 2005 IS AN INCOMPLETE YEAR
#### -- total count of distinct product line 
SELECT COUNT(DISTINCT PRODUCTLINE)                 </br>
FROM SalesDataSample..sales_data_sample                 </br>
#### -- THERE ARE 7 PRODUCT LINE                 </br>
#### -- total count of distinct product code    
SELECT COUNT(DISTINCT PRODUCTCODE)                 </br>
FROM SalesDataSample..sales_data_sample                 </br>
#### -- THERE ARE 109 PRODUCT CODE   
#### -- total count of distinct city of customers 
SELECT COUNT(DISTINCT CITY)                                                  </br>
FROM SalesDataSample..sales_data_sample                                                  </br>
#### -- THERE ARE 73 CITIES                                                  </br>
#### -- total count of distinct country of customer       
SELECT COUNT(DISTINCT COUNTRY)                                                  </br>
FROM SalesDataSample..sales_data_sample                                                  </br>
#### -- THERE ARE 19 COUNTRIES                 
#### -- total count of distinct deal size     
SELECT COUNT(DISTINCT DEALSIZE)                                                  </br>
FROM SalesDataSample..sales_data_sample                                                  </br>
#### -- THERE ARE 3 DEALSIZE    

</br>

#### -- ANALYSIS                                                  </br>
####  -- order line by sum of sales and count of order                                                </br>
SELECT ORDERLINENUMBER AS orderLineNumber, SUM(SALES) AS totalSales, COUNT(ORDERNUMBER) AS totalOrder                                                </br>
FROM SalesDataSample..sales_data_sample                                                </br>
GROUP BY orderLineNumber                                                </br>
ORDER BY totalSales DESC                                                </br>
#### -- ORDER LINE NUMBER 1 HAS THE MAXIMUM TOTAL SALES OF 1,119,219.21 AND TOTAL ORDER OF 307 UNITS                                                </br>
#### -- ORDER LINE NUMBER 18 HAS THE MINIMUM TOTAL SALES OF 24,155.62 AND TOTAL ORDER OF 10 UNITS                                                </br>
#### -- status by sum of sale and count of order                                                </br>
SELECT STATUS AS [status], SUM(SALES) AS totalSales, COUNT(ORDERNUMBER) AS totalOrder                                                </br>
FROM SalesDataSample..sales_data_sample                                                </br>
GROUP BY STATUS                                                </br>
ORDER BY totalSales DESC                                                </br>
#### -- SHIPPED HAS THE MAXIMUM TOTAL SALES OF 9291501.079 AND TOTAL ORDER OF 2617 UNITS                                               
#### -- DISPUTED HAS THE MINIMUM TOTAL SALES OF 72212.86 AND TOTAL ORDER OF 14 UNITS                                                                                   

#### -- year by sum of sale and count of order                                
SELECT YEAR_ID AS [year], SUM(SALES) AS totalSales, COUNT(ORDERNUMBER) AS totalOrder                                                </br>
FROM SalesDataSample..sales_data_sample                                                </br>
GROUP BY YEAR_ID                                                </br>
ORDER BY totalSales ASC                                                </br>
#### -- THERE WAS AN INCREASE IN SALE AND TOTAL ORDER BETWEEN 2003 AND 2004. HOWEVER DUE TO INCOMPLETE DATA MONTH FOR YEAR 205,                             
#### -- IT WOULD BE DIFFICULT TO SAY IF THERE WOULD BE A DECREASE OR INCREASE IN SALES.                                                </br>

#### -- use mean to predict if sales and total order would increase in year 2005                                                   </br>                                            
SELECT YEAR_ID AS [year], AVG(SALES) AS meanSales, AVG(ORDERNUMBER) AS meanOrder                                                </br>
FROM SalesDataSample..sales_data_sample                                                </br>
GROUP BY YEAR_ID                                                </br>
ORDER BY meanSales ASC                                                </br>
#### -- THERE ARE CHANCES THAT THERE WOULD ALSO BE AN INCREASE IN SALES AND TOTAL ORDER BETWEEN THE YEAR 2004 AND 2005                                               

#### -- months by sum of sale and count of order                                                </br>
#### -- NOTE: IN ORDER TO AVIOD BIAS IN DATA WE WOULD EXCLUDE YEAR 2005                                             
SELECT MONTH_ID AS MONTH, SUM(SALES) AS totalSales, COUNT(ORDERNUMBER) AS totalOrder                                                </br>
FROM SalesDataSample..sales_data_sample                                                </br>
WHERE YEAR_ID != 2005                                                </br>
GROUP BY MONTH_ID                                                </br>
ORDER BY totalSales DESC                                                </br>
#### -- MONTH 11 HAS THE MAXIMUM TOTAL SALES OF 2118885.67 AND TOTAL ORDER OF 597 UNITS                                               
#### -- MONTH 3 HAS THE MINIMUM TOTAL SALES OF 380238.63 AND TOTAL ORDER OF 106 UNITS                                             

#### -- FURTHER ANALYSIS ON THE PRODUCT LINE SOLD IN THE 11 MONTH                                                
SELECT MONTH_ID AS MONTH, PRODUCTLINE AS productLine, SUM(SALES) AS totalSales, COUNT(ORDERNUMBER) AS totalOrder                                                </br>
FROM SalesDataSample..sales_data_sample                                                </br>
WHERE YEAR_ID != 2005 AND MONTH_ID = 11                                                </br>
GROUP BY MONTH_ID, PRODUCTLINE                                                </br>
ORDER BY totalSales DESC                                                </br>
#### -- CLASSIC CARS HAS THE MAXIMUM TOTAL SALES OF 825156.26 AND TOTAL ORDER OF 219 UNITS                                               
#### -- TRAINS HAS THE MINIMUM TOTAL SALES OF 44794.63 AND TOTAL ORDER OF 15 UNITS                                              

#### -- product line by sum of sale and count of order                                             
SELECT PRODUCTLINE AS productLine, SUM(SALES) AS totalSales, COUNT(ORDERNUMBER) AS totalOrder                                                </br>
FROM SalesDataSample..sales_data_sample                                                </br>
GROUP BY PRODUCTLINE                                                </br>
ORDER BY totalSales DESC                                                </br>
#### -- CLASSIC CARS HAS THE MAXIMUM TOTAL SALES OF 3919615.66 AND TOTAL ORDER OF 967 UNITS                                             
#### -- TRAINS HAS THE MINIMUM TOTAL SALES OF 226243.47 AND TOTAL ORDER OF 77 UNITS                                               

#### -- top 10 city by sum of sale and count of order                                              
SELECT TOP 10 CITY AS city, SUM(SALES) AS totalSales, COUNT(ORDERNUMBER) AS totalOrder                                                </br>
FROM SalesDataSample..sales_data_sample                                                </br>
GROUP BY CITY                                                </br>
ORDER BY totalSales DESC                                                </br>
#### -- MADRID HAS THE MAXIMUM TOTAL SALES OF 1082551.44 AND TOTAL ORDER OF 304 UNITS                                               
#### -- BRICKHAVEN HAS THE MINIMUM TOTAL SALES OF 165255.2 AND TOTAL ORDER OF 47 UNITS                                             

#### -- country by sum of sale and count of order                                               
SELECT COUNTRY AS country, SUM(SALES) AS totalSales, COUNT(ORDERNUMBER) AS totalOrder                                                </br>
FROM SalesDataSample..sales_data_sample                                                </br>
GROUP BY COUNTRY                                                </br>
ORDER BY totalSales DESC                                                </br>
#### -- USA HAS THE MAXIMUM TOTAL SALES OF 3627982.83 AND TOTAL ORDER OF 1004 UNITS                                               
#### -- IRELAND HAS THE MINIMUM TOTAL SALES OF 57756.43 AND TOTAL ORDER OF 16 UNITS                                                

#### -- dealsize by sum of sale and count of order                                           
SELECT DEALSIZE AS dealSize, SUM(SALES) AS totalSales, COUNT(ORDERNUMBER) AS totalOrder                                                </br>
FROM SalesDataSample..sales_data_sample                                                </br>
GROUP BY DEALSIZE                                                </br>
ORDER BY totalSales DESC                                                </br>
#### -- MEDIUM SIZE HAS THE MAXIMUM TOTAL SALES OF 6087432.24 AND TOTAL ORDER OF 1384 UNITS                                           
#### -- LERGE SIZE HAS THE MINIMUM TOTAL SALES OF 1302119.26 AND TOTAL ORDER OF 157 UNITS                                                

#### -- BEST CUSTOMER USING RFM                                               

#### -- The “RFM” in RFM analysis stands for recency, frequency and monetary value.                                               
#### -- An RFM model is built using three key factors:                                              
#### -- How recently a customer has transacted with a brand                                              
#### -- How frequently they’ve engaged with a brand                                                
#### -- How much money they’ve spent on a brand’s products and services                                            
DROP TABLE IF EXISTS #rfm;                                                </br>
WITH                                                 </br>
rfm AS                                                </br>
(                                                </br>
	SELECT	CUSTOMERNAME AS customerName, SUM(SALES) AS monetaryValue, COUNT(ORDERNUMBER) AS frequency,                                                </br>
			MAX(ORDERDATE) AS lastDateOrder, (SELECT MAX(ORDERDATE) FROM SalesDataSample..sales_data_sample) AS maxDateOrder,                                        </br>
			DATEDIFF(DD, MAX(ORDERDATE), (SELECT MAX(ORDERDATE) FROM SalesDataSample..sales_data_sample)) AS recency                                                </br>
	FROM SalesDataSample..sales_data_sample                                                </br>
	GROUP BY CUSTOMERNAME                                                </br>
),                                                </br>
rfm_calc AS                                                </br>
(                                                </br>
	SELECT rfm.*,                                                </br>
			NTILE(4) OVER (ORDER BY recency DESC) AS rfmRecency,                                                </br>
			NTILE(4) OVER (ORDER BY frequency) AS rfmFrequency,                                                </br>
			NTILE(4) OVER (ORDER BY monetaryValue) AS rfmMonetaryValue                                                                                              </br>  </br>                                              
	FROM rfm                                                </br>
)                                                </br>
SE                                                </br>LECT rfm_calc.*,                                                </br>
		rfmRecency + rfmFrequency + rfmMonetaryValue AS rfmCell,                                                </br>
		CAST(rfmRecency AS VARCHAR) + CAST(rfmFrequency AS VARCHAR) + CAST(rfmMonetaryValue AS VARCHAR) AS rfmCellString                                      </br>
INTO #rfm                                                </br>
FROM rfm_calc                                                </br>
                                                </br>
SELECT CUSTOMERNAME, rfmRecency, rfmFrequency, rfmMonetaryValue,                                                 </br>
		CASE                                                </br>
			WHEN rfmCell >= 10 THEN 'loyal customers'                                                </br>
			WHEN rfmCell >= 8 THEN 'active customers'                                                </br>
			WHEN rfmCell >= 6 THEN 'potential customers'                                                </br>
			WHEN rfmCell >= 5 THEN 'new customers'                                                </br>
			ELSE 'lost customers'                                                </br>
		END rfmSegment                                                </br>
FROM #rfm                                                </br>
                                                </br>
                                                
