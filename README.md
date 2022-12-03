# Project 2 README
1. Names:
- Wil Hoppin, James Yang

2. UNIs:
- wwh2121, dy2471

3. PostgreSQL account info:
- psql -U dy2471 -h 34.75.94.195 -d proj1part2
- id: dy2471, pw: 9989

4. Description of all expansions:
- The first expansion we did was converting our Item description from a VARCHAR attribute to a TEXT attribute. Our reasoning behind this was to create a more advanced search on the home screen, where when users enter a string to search for an Item, we conduct a full-text search using tsvectors and tsqueries across the descriptions of the items and come up with relevant matches. Currently, we only filter for exact matches within the Item names of VARCHAR type using the LIKE operator.
- The second expansion we did was to create a new ARRAY attribute in the Items table named liked_by. This new attribute stores a sequence of user_id strings, indicating which users have liked this item. Before this addition, users could like an item as many times as they wanted because we were not tracking which users liked which items on the backend. Now, once users have already liked an item they cannot like it again, which we plan to indicate on the frontend of the application by making the heart icon red instead of gray.
- The third expansion we made to our database was turning the address attibute in the Users table into a composite type instead of a VARCHAR type. This allowed us to standardize the way we store dates into a series of VARCHAR values: house_number, street_name, street_type, city, state, zip. Before, a user could enter anything they wanted to as a string which elevated the risk of missing certain data types or otherwise having an unusable address.

5. Queries
- Homepage search event -> When a user searches for an item on the home page, we first look for VARCHAR exact string matches in the titles, then conduct a full-text search on the item descriptions using the FREETEXT function. For demo purposes, we assume form.query is a text string extracted from the search bar, which is an HTML input field.
    - SELECT *
    - FROM Items
    - WHERE UPPER(item_name) LIKE UPPER(form.query)
    - OR FREETEXT (description, form.query)
- New like event -> When a user likes an item, we add their user_id to the "liked_by" ARRAY attribute in the Items table. In the example below, we use the variables current_user and current_item for the user and item in question. An example value of current_user.user_id could be 10000000001 and an example value of current_item.item_id could be 10000000003.
    - UPDATE Items
    - SET liked_by = array_append(liked_by, current_user.user_id)
    - WHERE item_id = current_item.item_id
- Create new user event -> When a user is created, we add their address as an ARRAY instead of one VARCHAR element. For the sake of this query, we assume the "form" variable is JSON data populated from an HTML form on the signup page.
    - INSERT INTO Users (user_id, user_name, email, created_at, photo,address, venmo, favorite)
    - VALUES (form.userid, form.username, form.email, date.today(), form.photo, array[form.house_number, form.street_name, form.street_type, form.city, form.state, form.zip], form.venmo, 0)


