# The Silent Server – Backend Debugging Assignment
  
The objective was to debug the backend and restore the full authentication flow including OTP verification, session cookies, and JWT authentication.

---

## Tech Stack

- Node.js
- Express.js
- JSON Web Tokens (JWT)
- Cookie Parser
- Curl for API testing

---

## Project Structure

auth_assignment
│
├── server.js
├── middleware
│   ├── auth.js
│   └── logger.js
│
├── utils
│   ├── mockDb.js
│   └── tokenGenerator.js
│
├── package.json
├── output.txt
└── README.md

---

## Setup Instructions

Install dependencies:

npm install

Start the server:

npm start

Server runs at:

http://localhost:3000

---

## Authentication Flow

### 1. Login

Endpoint:

POST /auth/login

Example command:

curl -X POST http://localhost:3000/auth/login \
-H "Content-Type: application/json" \
-d '{"email":"your_email@example.com","password":"password123"}'

Response:

{
 "message": "OTP sent",
 "loginSessionId": "abc123"
}

OTP is printed in the server console.

---

### 2. Verify OTP

Endpoint:

POST /auth/verify-otp

Command:

curl -c cookies.txt -X POST http://localhost:3000/auth/verify-otp \
-H "Content-Type: application/json" \
-d '{"loginSessionId":"abc123","otp":"123456"}'

This generates a session cookie called session_token.

---

### 3. Generate Access Token

Endpoint:

POST /auth/token

Command:

curl -b cookies.txt -X POST http://localhost:3000/auth/token

Response:

{
 "access_token": "...",
 "expires_in": 900
}

---

### 4. Access Protected Route

Endpoint:

GET /protected

Command:

curl -H "Authorization: Bearer <access_token>" \
http://localhost:3000/protected

Response:

{
 "message": "Access granted",
 "user": {...},
 "success_flag": "FLAG-..."
}

---

## Bugs Fixed

1. Logger middleware was missing next() which blocked requests.
2. OTP was generated but not printed to the console.
3. Token endpoint incorrectly read session from Authorization header.
4. cookie-parser middleware was missing.
5. Token generator swallowed errors due to an empty catch block.

---

## Final Result

After fixing the bugs:

- Login generates OTP
- OTP verification sets session cookie
- Session cookie exchanges for JWT token
- JWT allows access to protected route

---

## Submission

This repository contains:

- Fixed backend code
- output.txt containing command outputs
- README.md documentation

