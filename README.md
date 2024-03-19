## Codebook Content 
   - The first part is dedicated to reading JSON files, retrieving dictionary values, organizing them into a dataframe, and checking for data quality issues associated with each tables.
   - The second part involves queries and answers to business questions.
   - SQL dialect is SQLite 3
## [Summary] Data Quality Issue 
 ### Brands Table
   - There are too many missing values in the category, categoryCode, topBrand, and brandCode columns.
   - categoryCode appears to contain text values instead of categorical values and is almost identical to the category values, which increases redundancy.
   - brandCode contains inconsistent values, including numeric, text, and a combination of both.
 ### Reciepts Table
   - The barcodes column contains empty strings that are not recognized as null values.
   - There are multiple values in the barcodes, finalPrice, itemPrice, partnerItemId, and quantityPurchased columns, indicating that First Normal Form normalization is needed.
   - The data types for pointsEarned, totalSpent, finalPrice, itemPrice, and quantityPurchased are object. These need to be changed to numeric.
 ### Users Table
   - There are missing values in the lastLogin, signUpSource, state, and scan_ym columns.
   - Within the users table, 283 users are duplicated among the 495 data points.
## Communicate with Stakeholders
 ### Questions about the data
   - Good afternoon manager, I found data quality issues across three tables which mainly involves missing values, multiple values in one cell, and inconsistent values. Before I jump into technical parts, I need more business information regards to reward operation and brand/product hierarchy. Please refer to the full context below and I would appreciate a short meeting to gather information about reward operation and brand/product hierarchy. Thank you. 
 ### How I found data quality issues
   - I checked for 1) missing values, 2) inconsistent values, 3) irrelevant columns to drop, and 4) duplicates in every table.
 ### What I need to know to solve data quality issues
   - I need a better understanding of the business context. Does each receipt have unique point information? Can it be scanned twice to double the points? When are bonus points earned? Is
     there a scenario where points are not earned after scanning? Policies or methods for point operations are needed to organize receipts and rewards tables.
   - My assumption is that the rewards table is based on the receipts table and relates to the points earned from the scanned receipt. This information will support splitting the current
     receipts table to resolve multiple values column issue (First Normal Form and Second Normal Form normalization).
   - How does the  hierarchical breakdown from brand to barcode work? Would it be like brands --> products --> barcodes? This information is also needed to resolve multiple values column
     issue(normalization). 
 ### Other information to optimize data set
   - A list of brand codes, category codes, product codes (barcodes), and their associated IDs and names is important to answer business questions. Organized master data is needed for
     optimization.
   - Is an ID generated per receipt, user, and brand? The ID column should be renamed based on the receipt, user, and brand, like receipt_id, user_id, brand_id for clarification.
 ### Performance and scaling concerns
   - Establish clear data governance policies that define data quality standards and ownership responsibilities. This framework supports a culture of quality and accountability.
   - I will support the person who is responsible of data governance policies to define data quality standards.
## [Summary] Answers to Business Questions
 ### TOP 5 brands
 - What are the top 5 brands by receipts scanned for most recent month?
   - The most recent month in the receipts table is March, 2021.
   - However, due to missing barcode values in receipt table or non listed barcode values in the brand table, most of them have null values. Receipts scanned in Jan, 2021 partially contain brand name values.
  
 - How does the ranking of the top 5 brands by receipts scanned for the recent month compare to the ranking for the previous month?
   - Top 5 brands for Jan 2021 are as below (containing name values)
    1. Tostitos
    2. Swanson
    3. Cracker Barrel Cheese
    4. Prego
    5. Diet Chris Cola
 ### Average spending and and total items purchased by rewardsReceiptStatus
 - When considering average spend from receipts with 'rewardsReceiptStatus’ of ‘Accepted’ or ‘Rejected’, which is greater?
   - First of all, there is no value named 'Accepted'.
   - If comparing with 'Finished' status, average spending is 913 dollars which is approx. 34 times higher than 'Rejected' average spending of 27 dollars. (Assuming spending in 1 dollar unit.)
   - When considering total number of items purchased from receipts with 'rewardsReceiptStatus’ of ‘Accepted’ or ‘Rejected’, which is greater?
   - If comparing with 'Finished' status, total number of items purchased is 491,828 which is approx. 1,927 times higher than 'Rejected' total number of items purchased of 255.

 ### Most recent users spending and transactions
 - Which brand has the most spend among users who were created within the past 6 months?
   - Tostitos has the most spending of 15,785 dollars among users who were created within the past 6 months.
   - Which brand has the most transactions among users who were created within the past 6 months?
   - Tostitos has the most transactions of 23 among users who were created within the past 6 months.
 - Please refer that the create_ym has missing month/year values of  Oct/2020 and Sep/2020 which needs to be included in the past 6 months window. As a result, it only add up 4 months cumulative values.

  
