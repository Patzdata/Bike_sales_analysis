## Bike_sales_analysis

#### Project Overview 

This data is aimed at using Sql to answer some business questions on the sales of bikes by analyzing various aspects of the table ranging from the employees to sales.

#### Data source

The primary dataset used for the analysis is the "bike_sales.csv" file, containing detailed information about
 all that is to know about the analysis.

#### Tools 

- MYSQL (Explanatoratory data analysis)

#### Data preparation

  1. Data loading
  2. understanding the table and the type of dataset we have.

###
- Perfom data cleaning
- Exploratory Data analysis

#### Data Analysis

``` Products Analysis

-- Q1: Find the total number of products for each product category.
select PRODCATEGORYID, count(*) as Total_NUMBER
from products
group by PRODCATEGORYID
order by Total_NUMBER desc;

![Screenshot 2024-07-03 010640](https://github.com/user-attachments/assets/4e2b3af3-495e-4629-a431-3d64cd23d4c6)



-- Q2: List the top 5 most expensive products.
select PRODCATEGORYID, PRODUCTID, PRICE
from products
group by PRODCATEGORYID, PRODUCTID, PRICE
order by PRICE desc
limit 5;

-- Q3: List the total sales amount (gross) for each product category.
select P.PRODCATEGORYID, P.PRICE, sum(S.GROSSAMOUNT) AS TOTAL_SALES_GROSS
from salesorderitems S
left join products P on S.PRODUCTID = P.PRODUCTID
group by PRODCATEGORYID, PRICE, GROSSAMOUNT
order by TOTAL_SALES_GROSS DESC;

-- Q4: List the top 5 suppliers by total product sales.
select p.SUPPLIER_PARTNERID, count(s.NETAMOUNT)as total_product_sale, p.PRICE, s.BILLINGSTATUS
from products p
left join salesorders s on p.SUPPLIER_PARTNERID = s.PARTNERID
group by SUPPLIER_PARTNERID, PRICE, BILLINGSTATUS
order by total_product_sale desc
limit 5;

-- Q5: Find the total number of products created by each employee.
select E.EMPLOYEEID, concat(NAME_FIRST, " ", NAME_LAST ) AS FULL_NAME, P.CREATEDBY, COUNT(*) AS PRODUCT_CREATED
from products P
left join employees E on P.CHANGEDBY = E.EMPLOYEEID
group by P.CREATEDBY, FULL_NAME, EMPLOYEEID
order by EMPLOYEEID, PRODUCT_CREATED desc;

-- Q6: List the employees who have changed product details the most.
select E.EMPLOYEEID, concat(NAME_FIRST, " ", NAME_LAST ) AS FULL_NAME, count(P.CHANGEDBY) AS PRODUCT_DETAILS_CHANGE
from products P
left join employees E on P.CHANGEDBY = E.EMPLOYEEID
group by P.CHANGEDBY, FULL_NAME, EMPLOYEEID
order by PRODUCT_DETAILS_CHANGE desc;

-- Sales Orders Items Analysis

-- Q7: Calculate the total gross amount for each sales order.
select SALESORDERID, sum(GROSSAMOUNT) as total_gross_sale
from salesorders
group by SALESORDERID
order by total_gross_sale;

-- Q8: Find the sales order items for a specific product ID.

select SALESORDERITEM, QUANTITY, SALESORDERID
from salesorderitems
where PRODUCTID = 'HB-1175'
group by SALESORDERITEM, QUANTITY, SALESORDERID
order by QUANTITY desc;

-- Business Partners Analysis

-- Q9: How many business partners are there for each partner role?
select PARTNERROLE, count(PARTNERID) as no_business_partner
from businesspartners
group by PARTNERROLE
order by no_business_partner;

-- Employees Analysis

-- Q10: Find the number of employees for each sex.
SELECT SEX,COUNT(*) no_employee
FROM employees
GROUP BY sex;

-- Product Categories Analysis

-- Q11: List all product categories along with their descriptions.
select PT.SHORT_DESCR, P.PRODCATEGORYID
from productcategories P
left join productcategorytext PT on P.PRODCATEGORYID = PT.PRODCATEGORYID
group by PRODCATEGORYID, SHORT_DESCR
order by PRODCATEGORYID desc;

-- Q12: Find all products that belong to the 'Mountain Bike' category.

select *
from products P
left join productcategorytext PT 
ON P.PRODCATEGORYID = PT.PRODCATEGORYID
where SHORT_DESCR = 'Mountain Bike' ;

-- Addresses Analysis

-- Q13: Count the number of addresses in each country.
select COUNTRY,count(*) AS NO_OF_ADDRESS
from addresses A
left join employees E
ON A.ADDRESSID = E.ADDRESSID
group by COUNTRY;

-- Q16: List all addresses in a specific city, e.g., 'New York'.

select *
from addresses
where CITY IN ('New York');

-- Q17: Find the number of business partners in each region.
select  REGION, COUNT(*)AS NO_BUSINESS_PARTNER
from addresses A
left join employees E
ON A.ADDRESSID = E.ADDRESSID
group by REGION;

-- Q18: List all addresses associated with a specific business partner.
select CITY, STREET, PARTNERID
from addresses A
join businesspartners B
ON A.ADDRESSID = B.ADDRESSID
where PARTNERID =0100000008;

-- Q19: Find the number of business partners in each region.
select  REGION, COUNT(*)AS NO_BUSINESS_PARTNER
from addresses A
left join businesspartners B
ON A.ADDRESSID = B.ADDRESSID
group by REGION;

-- Sales Orders Analysis
-- Q20: Calculate the total gross amount for each sales organization.
select SUM(SS.GROSSAMOUNT) AS GROSS_SALE_ORG, PRODUCTID, QUANTITY
from salesorders SS
left join salesorderitems SO ON SS.SALESORDERID = SO.SALESORDERID
group by PRODUCTID, QUANTITY
order by GROSS_SALE_ORG desc;

-- Q21: Find the top 5 sales orders by net amount.
select SALESORDERID, NETAMOUNT
from salesorders
group by SALESORDERID, NETAMOUNT
order by SALESORDERID desc
limit 5;

-- Q22: How many sales orders were created in the year 2018?
select year(CREATEDAT) AS YEAR, COUNT(NETAMOUNT) AS SALES_ORDER
from salesorders
group by YEAR;


-- Combining Data from Multiple Tables
-- Q23: List the top 5 employees who have created the most sales orders.
SELECT e.EMPLOYEEID, COUNT(s.SALESORDERID) AS sales_count FROM employees e
LEFT JOIN salesorders s
ON e.EMPLOYEEID=s.CREATEDBY
GROUP BY EMPLOYEEID
ORDER BY sales_count DESC
LIMIT 5;

-- Q24: Find the total sales amount (gross) for each product category.
SELECT p.PRODUCTID,pt.SHORT_DESCR AS product_name,SUM(GROSSAMOUNT) AS total_gross_amount, p.PRODCATEGORYID
FROM salesorders s 
LEFT JOIN products p 
ON s.PARTNERID=p.SUPPLIER_PARTNERID
LEFT JOIN productcategorytext pt
ON p.PRODCATEGORYID=pt.PRODCATEGORYID
GROUP BY p.PRODUCTID, product_name, p.PRODCATEGORYID
ORDER BY total_gross_amount DESC;
```

