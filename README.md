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
- Text search
- Like event -> adding a name to the likes array
- Adding an address to the user table when we do that -> what does this look like?