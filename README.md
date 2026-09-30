<p align="center">
  <img src="TRENDY.png" alt="Trendy logo" width="260">
</p>

<h1 align="center">Trendy</h1>

<p align="center">
  A Twitter inspired microblogging social network, built as the final project for the WDI course
  at the Federal University of Rondônia (UNIR).
</p>

<p align="center">
  <img src="https://img.shields.io/badge/PHP-8.2%2B-777BB4?logo=php&logoColor=white" alt="PHP 8.2+">
  <img src="https://img.shields.io/badge/MySQL-MariaDB-4479A1?logo=mysql&logoColor=white" alt="MySQL">
  <img src="https://img.shields.io/badge/Eloquent-ORM-FF2D20?logo=laravel&logoColor=white" alt="Eloquent ORM">
  <img src="https://img.shields.io/badge/HTML5-CSS3-E34F26?logo=html5&logoColor=white" alt="HTML and CSS">
</p>

> **Project archived.** This is a 2024 academic project that is no longer maintained. The code is kept here as a
> record and as part of our portfolio.

## About

Trendy is a web application where users sign up, post short messages of up to 280 characters (the "trends"), like
other people's posts and manage their own profile. The back end is written in plain PHP, using the Eloquent ORM
(Laravel's database layer) as a standalone package through `illuminate/database`, with MySQL as the database.

The interface is in Portuguese.

## Features

**Accounts and authentication**
- Sign up with a check for usernames that are already taken
- Passwords stored as bcrypt hashes (`password_hash` and `password_verify`)
- Login and logout handled with PHP sessions
- Protected pages: the feed and the profile are only available to logged in users

**Feed**
- Post messages limited to 280 characters
- Timeline ordered from newest to oldest
- Like and unlike posts, with a like counter
- Edit your own posts, which then show an "(editado)" label
- Delete your own posts
- Emoji support (the connection uses `utf8mb4`)

**Profile**
- Change your username, with an availability check
- Change your password, with a confirmation field

**Roles**
- Every user has a role (`user` by default, or `admin`)
- Admins can delete posts from any user
- For admins, the navigation bar turns red to signal moderation mode

## Tech stack

| Layer | Technology |
| --- | --- |
| Back end | PHP 8.2 or newer |
| Data access | Eloquent ORM (`illuminate/database` 11.x) through Capsule |
| Database | MySQL or MariaDB |
| Front end | HTML5 and CSS3, no frameworks |
| Dependencies | Composer |

## Project structure

| File | Purpose |
| --- | --- |
| `index.html` | Login landing page |
| `login.php` | User authentication |
| `register.html` | Sign up form |
| `register.php` | Account creation |
| `feed.php` | Timeline: post, like, edit, delete and log out |
| `profile.php` | Username and password editing |
| `database.php` | Database connection setup (Capsule) |
| `User.php` | User model |
| `Tweet.php` | Post model |
| `like.php` | Like model |
| `criatabelas.sql` | Table creation script |
| `style.css` | Styles for the login, sign up and profile pages |
| `TRENDY.png` | Project logo |
| `composer.json`, `composer.lock` | PHP dependencies |

## Database

The `criatabelas.sql` script creates three tables in the `idw` database:

- **users**: `id`, `username` (unique), `password` (hash) and `cargo` (role)
- **tweets**: `id`, `user_id`, `username`, `content`, `is_edited`, `created_at` and `updated_at`
- **likes**: links a `tweet_id` to a `user_id`; likes are removed together with their post (`ON DELETE CASCADE`)

## Getting started

### Requirements

- PHP 8.2 or newer with the `pdo_mysql` extension
- MySQL or MariaDB
- Composer

XAMPP or Laragon are handy options, since they ship with PHP and MySQL.

### Steps

1. Clone the repository:
   ```bash
   git clone https://github.com/Wyllgner/trendy.git
   cd trendy
   ```

2. Install the dependencies (the `vendor` folder is already committed, but this makes sure it is up to date):
   ```bash
   composer install
   ```

3. Create the database and the tables:
   ```bash
   mysql -u root -p -e "CREATE DATABASE idw CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;"
   mysql -u root -p idw < criatabelas.sql
   ```

4. Update the credentials in `database.php` if your MySQL uses a different user, password or port:
   ```php
   'host'     => 'localhost:3306',
   'database' => 'idw',
   'username' => 'root',
   'password' => '',
   ```

5. Start PHP's built in server:
   ```bash
   php -S localhost:8000
   ```

6. Open `http://localhost:8000` in your browser, create an account and start posting.

### Making a user an admin

There is no screen for promoting users. Change the role directly in the database:

```sql
UPDATE users SET cargo = 'admin' WHERE username = 'your_username';
```

Then log out and log in again so the session picks up the new role.

## Known issues

Since the project is archived, these known issues are documented here but will not be fixed:

- The `id` column of the `likes` table has neither `AUTO_INCREMENT` nor a primary key. On MySQL servers running
  in strict mode, liking a post may fail; changing the column to `AUTO_INCREMENT PRIMARY KEY` solves it
- Deleting posts is only restricted in the interface; the server does not check whether the request comes from
  the author or an admin
- A leftover `var_dump($_SESSION)` runs when a new post is submitted in `feed.php`
- Forms do not use CSRF tokens
- Database credentials are hardcoded in `database.php` instead of being read from environment variables
- The `vendor` folder is committed; it could be ignored by Git and generated with `composer install`

## Authors

- **Samih Santos**
- **Wyllgner França**

Federal University of Rondônia (UNIR), 2024.
