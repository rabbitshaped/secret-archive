
# 🔐 Confessional Secrets App

A minimal anonymous-style confession app where users can submit “secrets”, view them, and manage them in a private archive. Built with Node.js, Express, PostgreSQL, and EJS.
Expanded from 9.6 Secrets Project from "The Complete Full-Stack Web Development Bootcamp" on Udemy by Angela Yu from the App Brewery. The design focuses on simplicity, atmospheric UI, and fast interactions (no unnecessary steps between writing and seeing your submission).

---

## ✨ Features

- User registration and login system (Passport.js)
- Submit anonymous-style “secrets”
- Immediate feedback after submission (view latest secret)
- Personal archive of all submitted secrets
- One-click deletion of secrets
- Empty-state UI for new users
- Google OAuth (if enabled in config)

---

## 🧠 Core Concept

Each user has a private collection of secrets. The app is designed so users interact with:
- the latest submitted secret
- or their personal archive

No public feed or cross-user visibility exists.

<img width="400" alt="Secret Archive Home" src="https://github.com/user-attachments/assets/d494fd4d-f4f7-4e58-812a-71c2a74c4592" />
<img width="400" alt="Register Screen" src="https://github.com/user-attachments/assets/32c5139d-3434-4472-89bf-db926fd5e51b" />
<img width="400" alt="Confession Example" src="https://github.com/user-attachments/assets/bf038c44-4ee2-4235-a3c3-dad0fa4c83e5" />
<img width="400" alt="Secrets registered in Vault" src="https://github.com/user-attachments/assets/7a2fe018-6ae5-4719-922f-f5db979cd17c" />


---

## 🗄️ Database Structure

The project uses **two PostgreSQL tables** with a **one-to-many relationship**:

### `users`
Stores registered users.

- `id` (primary key)
- `email`
- `password` (hashed)

### `secrets`
Stores user-submitted secrets.

- `id` (primary key)
- `secret_text`
- `user_id` (foreign key → users.id)

### Relationship
- One user → many secrets
- Each secret belongs to exactly one user

---

## 🔒 Data Integrity

- Foreign key constraints ensure referential integrity
- Prevents orphaned secrets if a user is deleted
- If a user is removed, their associated secrets are handled according to database constraints (recommended: `ON DELETE CASCADE`)

---

## ⚙️ Tech Stack

- Node.js
- Express.js
- PostgreSQL
- EJS templating
- Passport.js (local strategy)
- bcrypt

---

## 🧾 Current Behavior Notes

- Secrets are deleted instantly when the delete button is clicked
- No confirmation step is currently implemented (by design for simplicity)
- Archive view allows browsing all past secrets
- Empty state is shown when no secrets exist

---

## 🚧 Known UI / UX Notes

The current CSS is still evolving:
- Blur effects and hover reveal interactions are experimental and MESSY
- Archive layout may show visual artifacts depending on browser rendering
- Some polish and cleanup is planned 100%

---

## 💡 Future Improvements and Ideas

- Add delete confirmation modal (optional UX safeguard)
- Soft delete instead of instant deletion (restore feature)
- Better handling of deleted/empty archive states
- Pagination or lazy loading for large archives
- Improved accessibility for blur-reveal interactions
- Mobile hover alternatives (tap-to-reveal)
- Theming system (dark/light or mood-based themes)

---

## 🚀 Setup Instructions

### 1. Clone the repository
`git clone <repo-url>`
`cd <project-folder>`

### 2. Install dependencies
`npm install`

### 3. Create environment variables
Create a `.env` file in the root directory:

`DB_USER=your_db_user
DB_PASSWORD=your_db_password
DB_HOST=localhost
DB_PORT=5432
DB_NAME=your_db_name
SESSION_SECRET=your_session_secret`

### 4. Create database tables
Run this in PostgreSQL:

`CREATE TABLE users (
	id SERIAL PRIMARY KEY,
	email TEXT UNIQUE NOT NULL,
	password TEXT NOT NULL
);`

`CREATE TABLE secrets (
	id SERIAL PRIMARY KEY,
	secret_text TEXT NOT NULL,
	user_id INTEGER REFERENCES users(id) ON DELETE CASCADE
);`

### 5. Start the application 
```npm start```
Then open
`http://localhost:3000`
