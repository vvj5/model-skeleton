ActiveRecord Homework 5/27/2015


1) How many users are there?
    A: 50

    Code (ActiveRecord):  User.count

    Code (sqlite3)  SELECT COUNT id FROM users;


2) What are the 5 most expensive items?

    A: Small Cotton Gloves|9984
       Small Wooden Computer|9859
       Awesome Granite Pants|9790
       Sleek Wooden Hat|9390
       Ergonomic Steel Car|9341

    Code (ActiveRecord): Item.order(price: :desc).limit(5)

    Code (sqlite3): SELECT title, price FROM items ORDER BY price DESC LIMIT 5;


3a) What’s the cheapest book?

    A: Ergonomic Granite Chair

    Code (ActiveRecord): Item.where(category: 'Books').order("price ASC")

    Code (sqlite3): SELECT title, price FROM items WHERE category LIKE 'books' ORDER BY price ASC LIMIT 5;

        3b) Does that change for “category is exactly ‘book’”:

        A: No, it's the same answer.

        Code (ActiveRecord):

        Code (sqlite3): SELECT title, price FROM items WHERE category = 'books' ORDER BY price ASC LIMIT 5;


4a) Who lives at “6439 Zetta Hills, Willmouth, WY”?

    A: Corrine Little

    Code 1st:  SELECT user_id, street, city, state FROM addresses WHERE street = '6439 Zetta Hills';

    Code 2nd:  SELECT id, first_name, last_name FROM users WHERE id = '40';

        4b) Do they have another address?

        A: Yes - 54369 Wolff Forg  Lake Bryon  CA

        code: SELECT user_id, street, city, state FROM addresses WHERE user_id = '40';


5) Correct Virginie Mitchell’s address to “New York, NY, 10108”.

    Code 1st: SELECT id, first_name, last_name FROM users WHERE first_name = 'Virginie' AND last_name = 'Mitchell';

    Code 2nd: UPDATE addresses SET city = "New York", state = "NY", zip = "10108" WHERE user_id = "39";


6) How much would it cost to buy one of each tool?

    A: 7383

    Code: SELECT SUM(price) FROM items WHERE category LIKE "tools";


7) How many total items did we sell?

    A: 2215

    Code: SELECT SUM(quantity) FROM orders;


8) How much was spent on books?

    A:  in Items/categories, select books, then set item_id=price;
        in Orders, select books, multiply prices by quantity;
        sum of order amounts

    CODE SHOULD HAVE BEEN:
      SELECT SUM(price * quantity) FROM orders JOIN items ON items.id = orders.item_id AND category LIKE "%book%";


9) Simulate buying an item by inserting a User for yourself and an Order for that User.
    A: I worked and worked on this, but kept getting errors I couldn't figure out. Even restarted SQLITE3, no luck.

    Code: INSERT INTO users (first_name, last_name, email) VALUES ("Will", "Sherman", "vvj5aticlouddotcom");




What item was ordered most often? Grossed the most money?
What user spent the most?
What were the top 3 highest grossing categories?


























This folder structure should be suitable for starting a project that uses a database:

* Fork this repo
* Clone this repo
* `rake generate:migration <NAME>` to create a migration (Don't include the `<` `>` in your name, it should also start with a capital)
* `rake db:migrate` to run the migration and update the database
* Create models in lib that subclass `ActiveRecord::Base`
* ... ?
* Profit


## Rundown

```
.
├── Gemfile             # Details which gems are required by the project
├── README.md           # This file
├── Rakefile            # Defines `rake generate:migration` and `db:migrate`
├── config
│   └── database.yml    # Defines the database config (e.g. name of file)
├── console.rb          # `ruby console.rb` starts `pry` with models loaded
├── db
│   ├── dev.sqlite3     # Default location of the database file
│   ├── migrate         # Folder containing generated migrations
│   └── setup.rb        # `require`ing this file sets up the db connection
└── lib                 # Your ruby code (models, etc.) should go here
    └── all.rb          # Require this file to auto-require _all_ `.rb` files in `lib`
```
