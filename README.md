# Secrets — Authentication Level 1

A basic authentication assignment built with Node.js, Express, EJS, and PostgreSQL. Users register with an email and password, then log in to view a secret page.

This project demonstrates handling HTML forms, storing user records, querying a database, and checking credentials on the server.

## Features

- User registration with duplicate email checks.
- Login using credentials stored in PostgreSQL.
- A secret page displayed after successful registration or login.
- Parameterized SQL queries and error responses.
- EJS templates with shared partials and CSS styling.

## Setup

Install Node.js with npm and PostgreSQL, and make sure PostgreSQL is running.

1. Install dependencies from the project directory:

   ```bash
   npm install
   ```

2. Create the `secrets` database and its table using your local PostgreSQL role:

   ```bash
   psql -U YOUR_POSTGRES_USER -d postgres -c 'CREATE DATABASE secrets;'
   psql -U YOUR_POSTGRES_USER -d secrets -f queries.sql
   ```

   `queries.sql` creates the `users` table with `id`, `email`, and `password` columns. Email addresses must be unique.

3. Update the database configuration in `index.js` to match your local PostgreSQL credentials:

   ```js
   const db = new pg.Client({
     user: "YOUR_POSTGRES_USER",
     host: "localhost",
     database: "secrets",
     password: "YOUR_POSTGRES_PASSWORD",
     port: 5432,
   });
   ```

   The application reads these settings directly from `index.js`; it does not load a `.env` file.

4. Start the server:

   ```bash
   node index.js
   ```

5. Open [http://localhost:3000](http://localhost:3000).

## Usage and manual checks

1. Select **Register** and enter a new email and password. Successful registration displays the secret page.
2. Return to the home page, select **Login**, and enter the same credentials. Successful login displays the secret page.
3. Register the same email again to check the duplicate email response (`400`).
4. Log in with an incorrect password or an unregistered email to check the authentication failure responses (`401`).

Database or server errors during registration and login return `500`. No automated tests are configured; `npm test` currently exits with a placeholder error.

## Routes

| Method | Path | Purpose |
| --- | --- | --- |
| GET | `/` | Display the home page. |
| GET | `/register` | Display the registration form. |
| POST | `/register` | Create a user and render the secret page. |
| GET | `/login` | Display the login form. |
| POST | `/login` | Check credentials and render the secret page. |

Both forms submit the email in the `username` field and the password in the `password` field.

## Project structure

```text
index.js             Application entry point and authentication routes
solution.js          Reference implementation
queries.sql          Users table definition
views/               EJS pages and shared template partials
public/css/          Styles served by Express
package.json         Dependencies and package configuration
```

## Assignment scope

This Level 1 exercise stores passwords in plain text and compares them directly during login. User records, including passwords, are also logged to the console. This is a learning implementation and is unsuitable for production authentication.

There are no sessions, persistent login, logout, or protected GET route for the secret page. The page displays a fixed secret; submitting and storing secrets is outside the current implementation. Possible next steps include password hashing, session management, input validation, and moving database credentials out of source code.
