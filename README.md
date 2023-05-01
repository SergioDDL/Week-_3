# Week-_3_HW

What is our overall conversion rate?
`````  58%

WITH purchased_C AS
(
    SELECT 
    session_guid, 
    SUM(CASE WHEN event_type = 'package_shipped' THEN 1 ELSE 0 END) AS purchase_completed
    FROM DEV_DB.DBT_SERGIOD.STG_POSTGRES_EVENTS
    GROUP BY session_guid
)
    
SELECT SUM(purchase_completed)/count(purchase_completed) AS conversion_rate FROM purchased_C;


What is our conversion rate by product?

This is calculated  total by count for each product. 

WITH sessions_by_products AS
(
    SELECT 
    b.session_guid,
    b.product_guid,
    a.inventory
    FROM DEV_DB.DBT_SERGIOD.STG_POSTGRES_PRODUCTS a
    INNER JOIN DEV_DB.DBT_SERGIOD.STG_POSTGRES_EVENTS b
    ON a.product_guid = b.product_guid
    WHERE b.product_guid IS NOT NULL
),
    
sessions_by_products_count AS
(
    SELECT 
    aa.product_guid,
    bb.name,
    COUNT(aa.product_guid) AS purchases_product,
    SUM(bb.inventory) AS purchased_sessions,
    COUNT(bb.inventory) AS total_sessions,
    purchased_sessions/ total_sessions AS conversion_rate
    FROM sessions_by_products aa
    INNER JOIN DEV_DB.DBT_SERGIOD.STG_POSTGRES_PRODUCTS bb
    ON aa.product_guid = bb.product_guid
    GROUP BY aa.product_guid, bb.name
    )
    
SELECT * 
FROM sessions_by_products_count;



Part 3: We’re starting to think about granting permissions to our dbt models in our snowflake database so that other roles can have access to them.

Created macros to ensure values are not correctly above 0
created a macro to grant access to the data wharehouse by role



Part 4:  After learning about dbt packages, we want to try one out and apply some macros or tests.

created a macro that tests values and this macro can easily be convered in jinja to test:


Part 5: After improving our project with all the things that we have learned about dbt, we want to show off our work!

I am uploading the lineage graph in the project. done.

Part 6. dbt Snapshots

Pothos, Bamboo, Philodendron, Monstera, String of pearls, ZZ Plant




