
# Final-Project-Transforming-and-Analyzing-Data-with-SQL
Database used : ecommerce 
[Project presentation](https://prezi.com/view/C4VNQDXh0VXeMUvYeB3j/)
  

## Project/Goals

Use the skill and knowledge I have gained over the last 2 weeks in a "real life" like database from importing the data to making queries to answer questions and gain more insight 

  

## Process
Below is an outline of the process I went through during this project

### Reviewing CSVs and Importing
- Reviewed and did basic cleaning of raw data CSVs
	- Removed all columns with no values
	- Did a first pass for duplicates
- Imported all raw data CSVs
	- Set primary keys
	- Set constraints
	- Set column data types

### Data Exploration
- Explored tables
- Tested joining tables
- Experimented how data interacts
### Data Cleaning
- Removing Duplicates
- Removing ``NULLs`` and empty values
- Correcting values
- Reformatting columns
### Getting to Work!
- Starting With Questions
- Starting With Data
### QA
- Double checking results from Starting With Questions and Starting With Data


  

## Results

Highlights of some of the things learned through queries
- Majority of visitors and customers of the ecommerce website are located in the United States
	- There is a lot of interesting information within the ``all_sessions`` table, including location data about website views. Cross-referencing that with information in the ``sales_report`` table we can see that most views, some of which become customers, are based in the United States
- December is the most profitable month of the year
	- Looking into the ``sales_report`` table we can find the revenue for each month. As most of the customers are located in the United States, which is a country that is known to have large gift giving holidays in December  
- Apparel is the largest product category
	- Examining the ``v2categoryname`` in the``all_sessions`` table we can see many categories and sub categories. Using a ``COUNT()`` it is clear that apparel is the main product the ecommerce website
- Higher the cost of a product the more likely it is to be purchased
	- Comparing the ``unitprice`` and the numbers of products ordered a pattern appears. The top 50% of products are purchased much more than the lower cost products.

  

## Challenges

- My own lack of practice
	- More than once I made things more challenging for myself. For example I accidently set ``city`` = ``country`` and needed to completely reimport and reclean ``all_sessions``.
- Limited time
	- This project was done a tight deadline at the same time as another high priority school assignment. There is a number of things I would have liked to spend more time on. 
- Limited data
	- There is only so much data in a demo set like this one with no way to gather more. I could have gotten more and deep insight into the trends and patterns with more data.

  

## Future Goals

Future Todo List

 - [ ] Clean every column, not only the ones I needed to answer questions
 - [ ] Properly normalize the data
 - [ ] Create visualizations 
	 - [ ] Visitor vs customer locations
	 - [ ] Monthly revenue
