# Raamatukogu

This application is a simple library management system built with Node.js and Express. It allows users to manage books, users, and loans through a REST API.

## Tehnoloogiad

Technologies used:
- Node.js
- Express.js
- GitHub Actions
- JavaScript
- Docker

## Käivitamine

```bash
cd raamatukogu-lab

npm install

npm start
```

## Testikasutajad


| data | 321 |123|
|--------|-----|-----|
| Name | 321 | 123 | 
| Login | 321 | 123 |
| Password | 321 | 123 |


## API endpointid

### Kasutajad

| Meetod | URL | Kirjeldus |
|--------|-----|-----------|
| POST | /api/users/signup | create a new user |
| POST | /api/users/login | log into your account |
| POST | /api/users/logout | log out of your account |
| GET | /api/users/me | Returns the current logged in user's data. |

### Raamatud

| Meetod | URL | Kirjeldus |
|--------|-----|-----------|
| GET | /api/books | Returns a list of all books |
| GET | /api/books/:id | Returns a single book by ID |
| GET | /api/books/search | Searches for books by parameters (e.g. title or author) |
| GET | /api/books/genres | Returns a list of all available genres |
| GET | /api/books/genre/:genre | Tagastab kõik raamatud valitud žanri järgi |

### Laenud

| Meetod | URL | Kirjeldus |
|--------|-----|-----------|
| POST | /api/loans | Creates a new loan (borrows a book for a user) |
| POST | /api/loans/:id/return | Marks a loan as returned |
| GET | /api/loans | Returns a list of all loans |
| GET | /api/loans/me | Returns loans for the currently authenticated user |

## Testid

```bash
npm test
```

## GitHub Actions

GitHub Actions automatically runs tests and code reviews every time a push or pull request is made to the main branch.

The process installs dependencies, starts the server and tests, and checks the code for syntax. If all steps pass, the build is marked as successful.
