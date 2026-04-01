# Maestro Backend

## About The Project

Maestro is a music presentation and management platform for a composer and their clients. The goal of this platform is to showcase a musical portfolio, present services, and provide a secure space for communication and project tracking.

With Maestro, the composer has a showcase site and a management tool, allowing them to present their works, administer their catalog, and maintain contact with their clients. For clients, it is a personal space to exchange with the composer and follow the progress of their projects. For visitors, Maestro is a musical showcase where one can discover the composer, listen to excerpts, and explore their artistic universe. The entire system has been designed to offer a fluid, simple, and accessible experience, whether on a computer, tablet, or smartphone.

This repository contains the backend service for the Maestro application, built with Node.js, Express, and Sequelize.

## Getting Started

Follow these instructions to get a copy of the project up and running on your local machine for development and testing purposes.

### Prerequisites

*   Node.js (v18 or higher)
*   npm
*   PostgreSQL

### Installation

1.  **Clone the repository**
    ```sh
    git clone https://github.com/malvina-dev/maestro-back.git
    cd maestro-back
    ```

2.  **Install NPM packages**
    ```sh
    npm install
    ```

3.  **Set up the environment variables**

    Create a `.env` file in the root directory and populate it based on the `.env.example` file.
    ```
    PORT=3000
    PG_URL=postgresql://<user>:<password>@<host>:<port>/<database>
    JWT_SECRET=your_jwt_secret_key
    REFRESH_SECRET=your_jwt_refresh_secret_key
    CLIENT_URL=http://localhost:5173 
    ```

4.  **Set up the PostgreSQL database**

    Connect to your PostgreSQL instance and run the following commands to create the database and user:
    ```sql
    CREATE USER your_user WITH PASSWORD 'your_password';
    CREATE DATABASE your_database OWNER your_user;
    ```
    *Ensure the credentials match your `PG_URL` in the `.env` file.*

5.  **Seed the database**

    Run the seeding script to populate the database with initial data.
    ```sh
    npm run seed
    ```

6.  **Run the application**

    Start the server in development mode with auto-reloading.
    ```sh
    npm run dev
    ```
    The server will be running on `http://localhost:3000`.

## Tech Stack

*   **Backend:** Node.js, Express.js
*   **Database:** PostgreSQL
*   **ORM:** Sequelize
*   **Authentication:** JSON Web Tokens (JWT), bcryptjs
*   **File Handling:** Multer for file uploads
*   **Security:** `express-rate-limit`, `sanitize-html`, CORS

## API Endpoints

The API provides endpoints for managing users, projects, musical previews, and more. Authentication is required for most client and admin routes.

### Authentication

| Method | Endpoint             | Description                       |
| :----- | :------------------- | :-------------------------------- |
| `POST` | `/api/user`          | Register a new user.              |
| `POST` | `/api/user/login`    | Log in and receive auth tokens.   |
| `POST` | `/api/user/logout`   | Log out and clear auth cookies.   |
| `POST` | `/api/user/refresh`  | Refresh the access token.         |

### Users

| Method  | Endpoint                | Description                                     | Access       |
| :------ | :---------------------- | :---------------------------------------------- | :----------- |
| `GET`   | `/api/user/profile`     | Get the current user's profile.                 | Authenticated|
| `PATCH` | `/api/user`             | Modify the current user's information.          | Authenticated|
| `GET`   | `/api/admin/user`       | Get a list of all users.                        | Admin        |
| `GET`   | `/api/admin/user/filter`| Sort users by `lastname` or `firstname`.        | Admin        |
| `PATCH` | `/api/admin/user/:id`   | Deactivate or reactivate a user account.        | Admin        |

### Projects

| Method  | Endpoint                   | Description                                  | Access       |
| :------ | :------------------------- | :------------------------------------------- | :----------- |
| `POST`  | `/api/project`             | Client creates a new project request.        | Authenticated|
| `GET`   | `/api/project`             | Client lists their own projects.             | Authenticated|
| `GET`   | `/api/project/filter`      | Client filters their projects by status.     | Authenticated|
| `GET`   | `/api/admin/project`       | Admin lists all projects.                    | Admin        |
| `GET`   | `/api/admin/project/filter`| Admin filters all projects by status.        | Admin        |
| `PATCH` | `/api/admin/project/:id`   | Admin updates a project's status.            | Admin        |
| `DELETE`| `/api/admin/project/:id`   | Admin deletes a project.                     | Admin        |

### Portfolio & Content

| Method   | Endpoint                  | Description                                            | Access |
| :------- | :------------------------ | :----------------------------------------------------- | :----- |
| `GET`    | `/api/preview`            | Get all musical previews with their genres.            | Public |
| `GET`    | `/api/preview/star`       | Get all featured ("star") previews.                    | Public |
| `GET`    | `/api/preview/filter`     | Filter previews by genre or sort by date.              | Public |
| `POST`   | `/api/admin/preview`      | Admin adds a new preview (audio file upload).          | Admin  |
| `PATCH`  | `/api/admin/preview/:id`  | Admin updates a preview's details.                     | Admin  |
| `DELETE` | `/api/admin/preview/:id`  | Admin deletes a preview and its associated file.       | Admin  |
| `GET`    | `/api/genre`              | Get all musical genres.                                | Public |
| `POST`   | `/api/admin/genre`        | Admin creates a new genre.                             | Admin  |
| `PATCH`  | `/api/admin/genre/:id`    | Admin updates a genre.                                 | Admin  |
| `DELETE` | `/api/admin/genre/:id`    | Admin deletes a genre.                                 | Admin  |
| `GET`    | `/api/description`        | Get all portfolio description sections.                | Public |
| `POST`   | `/api/admin/description`  | Admin adds a new description (image upload).           | Admin  |
| `PATCH`  | `/api/admin/description/:id`| Admin updates a description and its image.           | Admin  |
| `DELETE` | `/api/admin/description/:id`| Admin deletes a description.                         | Admin  |

### Contact Messages

| Method   | Endpoint                | Description                               | Access |
| :------- | :---------------------- | :---------------------------------------- | :----- |
| `POST`   | `/api/message-contact`  | Submits a new contact message from a visitor. | Public |
| `GET`    | `/api/message-contact`  | Admin retrieves all contact messages.         | Admin (implied) |
| `PATCH`  | `/api/message-contact/:id`| Admin updates a message's status (e.g., "read").| Admin (implied) |
| `DELETE` | `/api/message-contact/:id`| Admin deletes a message.                      | Admin (implied) |

## Project Structure

```
maestro-back/
├── app/
│   ├── controllers/  # Handles request logic
│   ├── db/           # Database connection and seeding
│   ├── middlewares/  # Express middlewares (auth, file uploads)
│   ├── models/       # Sequelize model definitions and associations
│   └── routers/      # API route definitions
├── imageUploads/     # Directory for uploaded description images
├── uploads/          # Directory for uploaded audio previews
├── index.js          # Main application entry point
├── package.json      # Project dependencies and scripts
└── .env.example      # Example environment variables
```

## Security

Several security measures have been implemented to protect the application and its users:

*   **Password Hashing:** User passwords are securely hashed using `bcryptjs` before being stored in the database.
*   **Authentication:** API access is controlled using JSON Web Tokens (JWT). Access and refresh tokens are stored in secure, `httpOnly` cookies to mitigate XSS attacks.
*   **Authorization:** Routes are protected using middleware that checks for user authentication and role (e.g., administrator).
*   **Input Sanitization:** User-provided input is sanitized using `sanitize-html` to prevent Cross-Site Scripting (XSS) attacks.
*   **Rate Limiting:** The `express-rate-limit` middleware is used to limit repeated requests to public endpoints, helping to prevent brute-force attacks.
*   **CORS:** Cross-Origin Resource Sharing is configured to only allow requests from the designated client application URL.
