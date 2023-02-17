Welcome to the Ekin wiki!

# Ekin Design Documents

[Ekin Live]()

+ [Feature List](feature-list)
+ [Schema](schema)

#  Feature List

Ekin, a NIke clone, is an e-commerce application that allows users to shop an assortnment of fitness equipment.

### 1. Hosting (0x/0x/20xx)

### 2. New account creation, login, and guest/demo login (04/04, 2 days)
  + Users can sign up, sign in, log out
  + Users can use a demo login to try the site
  + Users can't use certain features without logging in (creating chirps & likes)

### 3. Reviews (0x/0x, 2 days)
  + Logged in users can create reviews
  + Users can view a list reviews
  + Logged in users can edit existing reviews

### 4. Favorites (0x/xx, 2 days)
  + Logged in users can favorite items

### 6. Search Itmes (0x/xx, 1 day)
  + Users can search for items through text to see results for that item

### 7. Production README (0x/xx, 0.5 days)

# Postgres Database Schema

## `users`
| column name       | data type | details                   |
|:------------------|:---------:|:--------------------------|
| `id`              | bigint    | not null, primary key     |
| `username`        | string    | not null, indexed, unique |
| `email`           | string    | not null, indexed, unique |         
| `password_digest` | string    | not null                  |
| `session_token`   | string    | not null, indexed, unique |
| `created_at`      | datetime  | not null                  |
| `updated_at`      | datetime  | not null                  |

+ index on `username, unique: true`
+ index on `email, unique: true`
+ index on `session_token, unique: true`
+ `has_many reviews`
+ `has_many favorites`
  
## `reviews`
| column name          | data type | details                        |
|:---------------------|:---------:|:-------------------------------|
| `id`                 | bigint    | not null, primary key          |
| `body`               | string    | not null                       |
| `author_id`          | integer   | not null, indexed, foreign key |
| `created_at`         | datetime  | not null                       |
| `updated_at`         | datetime  | not null                       |

+ `author_id` references `users`
+ index on `author_id`
+ `belongs_to author`
  
## `favorites`
| column name       | data type | details                        |
|:------------------|:---------:|:-------------------------------|
| `id`              | bigint    | not null, primary key          |
| `user_id`         | bigint    | not null, indexed, foreign key |
| `item_id`        | integer   | not null, indexed, foreign key |             
| `created_at`      | datetime  | not null                       |
| `updated_at`      | datetime  | not null                       |

+ `user_id` references `users`  
+ `review_id` references `reviews`
+ index on `[:review_id, :user_id], unique: true`
+ `belongs_to review`
+ `belongs_to user`
