
# EasyShop Update

This is an update for the simulated EasyShop website, in which I make various improvements to the backend API.

### Changes made:
- Added the ability for the user to view the list of product categories.
- Added ability for users with admin privileges to add, update, and delete categories.
- Fixed bug where the product price search function was not functioning correctly.
- Fixed bug where updating the product list was sometimes producing repeat item entries.
- Shopping cart functionality implemented.
    - User can press "Add to Cart" on a product to add it to their cart.
    - If the cart contains the same product already, adds one to the quantity of that product.
    - User can empty their cart.

### Screenshots
![EasyShop Home.PNG](screenshots/EasyShop%20Home.PNG)
 ![EasyShop API Start.PNG](screenshots/EasyShop%20API%20Start.PNG)
### Code Highlight

It was an interesting experience to implement the shopping cart system. I had a particularly difficult time with implementing the feature of increasing product quantity when multiples of the same item were added to a cart even though I already seemingly had the correct POST and PUT methods created. I should have realized that the Add to Cart buttons will only send POST requests and not automatically switch to PUT requests at an appropriate time without more adjustments in the frontend or backend. This is the solution I ended up implementing.

```
String sql = """
        MERGE INTO shopping_cart AS target
        USING (SELECT ? AS user_id, ? AS product_id) AS source
        ON target.user_id = source.user_id AND target.product_id = source.product_id
        WHEN MATCHED THEN
            UPDATE SET quantity = quantity + 1
        WHEN NOT MATCHED THEN
            INSERT (user_id, product_id, quantity)
            VALUES (source.user_id, source.product_id, 1);
        """;
```
This solution was developed with assistance from ChatGPT. Here's the relevant conversation log: https://chatgpt.com/share/685e808d-c188-8010-a34d-f73206ca41aa.

It was an educational experience to see this solution, as it taught me more about generally accepted REST practices (as this solution uses POST for a function usually reserved for PUT) as well as teaching me about MERGE: https://www.geeksforgeeks.org/merge-statement-sql-explained/.
