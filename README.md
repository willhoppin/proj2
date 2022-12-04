# Project 2 README
1. Names:
- Wil Hoppin, James Yang

2. UNIs:
- wwh2121, dy2471

3. PostgreSQL account info/commands:
- psql -U dy2471 -h 34.75.94.195 -d proj1part2
- id: dy2471, pw: 9989

4. Description of all expansions:
- The first expansion we did was converting our Item description from a VARCHAR attribute to a TEXT attribute. Our reasoning behind this was to create a more advanced search on the home screen, where when users enter a string to search for a member of the Items table, we conduct a full-text search across the descriptions of the items using the FREETEXT function and come up with relevant matches. Currently, we only filter for exact matches within the Item names of VARCHAR type using the LIKE operator.
- The second expansion we did was to create a new ARRAY attribute in the Items table named liked_by. This new attribute stores a sequence of user_id strings, indicating which users have liked this item. Before this addition, users could like an item as many times as they wanted because we were not tracking which users liked which items on the backend. Now, once users have already liked an item they cannot like it again, which we plan to indicate on the frontend of the application by making the heart icon red instead of gray.
- The third expansion we made to our database was adding a customer service table using a custom type. We create "cus_serv_type" as the primary data member to store each ticket as a singular custom type which contains additional information about the sending user we can display to receiving user. This has an added benefit of only requiring us to make 1 API call to get information about the users that wrote each ticket (otherwise, we would need to extract data from the Users table). To do this, we created a new table titled customer_service of custom type cus_serv_type, and another new table called "request" to model a relationship between the user table and the customer_service table to prevent dangling the customer_service table. This is similar to our "communicate" table that already exists in our database between the chats table and users table.

5. Queries
- Homepage search event -> When a user searches for an item on the home page, we look for both VARCHAR exact string matches with the item_name attribute in the Items table and full-text search matches on the item descriptions using the FREETEXT function. For the purpose of the below example query, we assume form.query is a text string extracted from the search bar, which is an HTML input field.
    - SELECT *
    - FROM Items
    - WHERE UPPER(item_name) LIKE UPPER(form.query)
    - OR FREETEXT (description, form.query)
- New like event -> When a user likes an item, we add their user_id to the new "liked_by" ARRAY attribute in the Items table. In the example below, we use the variables current_user and current_item for the user and item in question. An example value of current_user.user_id could be 10000000001 and an example value of current_item.item_id could be 10000000003.
    - UPDATE Items
    - SET liked_by = array_append(liked_by, current_user.user_id)
    - WHERE item_id = current_item.item_id
- Create a new customer service ticket event -> When a user requests service after purchasing a product, we register a ticket into our customer_service table as follows:
    - FIRST QUERY: CREATING A TYPE
        - CREATE TYPE cus_serv_type AS (
            - cs_id INT,
            - personal_info VARCHAR(20)ARRAY[3],
            - description TEXT
            - )
    - SECOND QUERY: CREATING A TABLE
        - CREATE TABLE customer_service of cus_serv_type
        - (PRIMARY KEY(cs_id))
    - THIRD QUERY: INSERTING A VALUE
        - INSERT INTO customer_service
        - VALUES 2,
        - ARRAY['michael','m','9297928273'],
        - 'I am very satisfied with the app. I am using a 10.1-inch Galaxy Tab, but the app comes out broken. The screen does not seem to be compatible.'


