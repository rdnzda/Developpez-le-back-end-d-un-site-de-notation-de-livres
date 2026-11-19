# Mon Vieux Grimoire – book rating API (back end)

**OpenClassrooms "Web Integrator" program – project 7.**

The back end of a book rating website: a REST API built with Node.js, Express and MongoDB. The front end was provided by OpenClassrooms.

## Features

- **Users**: sign-up and login, with passwords hashed with bcrypt and authentication through JSON Web Tokens (JWT)
- **Books**: list, view, create, update and delete books (create, update and delete require a valid token)
- **Ratings**: users rate books from 0 to 5, a user can rate a book only once, and the average rating is updated
- **Best rated**: an endpoint returning the top-rated books
- **Images**: uploads handled with Multer, then converted to WebP and optimized with Sharp

## API endpoints

| Method | Route | Auth | Description |
|---|---|---|---|
| POST | `/api/auth/signup` | – | Create an account |
| POST | `/api/auth/login` | – | Log in and get a token |
| GET | `/api/books` | – | List all books |
| GET | `/api/books/bestrating` | – | Top-rated books |
| GET | `/api/books/:id` | – | Get one book |
| POST | `/api/books` | ✅ | Create a book (with an image) |
| PUT | `/api/books/:id` | ✅ | Update a book |
| DELETE | `/api/books/:id` | ✅ | Delete a book |
| POST | `/api/books/:id/rating` | ✅ | Rate a book |

## Tech stack

- Node.js, Express
- MongoDB with Mongoose (and `mongoose-unique-validator`)
- bcrypt, jsonwebtoken
- Multer, Sharp

## Getting started

1. Copy `.env.example` to `.env` and fill in your MongoDB connection string and a long random JWT secret. `.env` is ignored by Git: never commit it.
2. Install the dependencies and start the API (Node.js 20.6 or later, which loads `.env` natively):

   ```bash
   npm install
   npm start
   ```

## Security review (2026)

While re-reading this project as part of my cybersecurity training, I found four issues:

1. **Broken access control** (OWASP A01): the update and delete routes check that the user is logged in, but not that the book belongs to them.
2. **Rating spoofing**: the rating route reads the user ID from the request body instead of the verified token.
3. **Hard-coded JWT secret** ✅ *fixed*: the token signing key is now read from the `JWT_SECRET` environment variable.
4. **Hard-coded database credentials** ✅ *fixed*: the connection string is now read from the `MONGODB_URI` environment variable, and the exposed password has to be rotated since it remains in the Git history.

Issues 1 and 2 are still open.
