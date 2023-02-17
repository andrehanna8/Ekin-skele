Welcome to the bluebird wiki!

# Bluebird Design Documents

[Bluebird Live]()

+ [Feature List](feature-list)
+ [Schema](schema)

#  Feature List

Bluebird, a Twitter clone, is a social media application that allows users to publicly share short snippets of text that can be viewed and liked by other users.

### 1. Hosting (04/03/20xx)

### 2. New account creation, login, and guest/demo login (04/04, 2 days)
  + Users can sign up, sign in, log out
  + Users can use a demo login to try the site
  + Users can't use certain features without logging in (creating chirps & likes)

### 3. Chirps (04/06, 2 days)
  + Logged in users can create chirps
  + Users can view a list chirps
  + Logged in users can edit existing chirps

### 4. Likes (04/10, 2 days)
  + Logged in users can 'like' chirps
  + The like count is displayed for each chirp

### 5. Dashboard and Profile (04/11, 1 day)
  + Users have a private dashboard of other user's chirps
  + Users have a public profile of their own chirps

### 6. Search Users (04/12, 1 day)
  + Users can search for other users through text to see that user's profile

### 7. Production README (04/13, 0.5 days)

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
+ `has_many chirps`
+ `has_many likes`
  
## `chirps`
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
  
## `likes`
| column name       | data type | details                        |
|:------------------|:---------:|:-------------------------------|
| `id`              | bigint    | not null, primary key          |
| `user_id`         | bigint    | not null, indexed, foreign key |
| `chirp_id`        | integer   | not null, indexed, foreign key |             
| `created_at`      | datetime  | not null                       |
| `updated_at`      | datetime  | not null                       |

+ `user_id` references `users`  
+ `chirp_id` references `chirps`
+ index on `[:chirp_id, :user_id], unique: true`
+ `belongs_to chirp`
+ `belongs_to user`