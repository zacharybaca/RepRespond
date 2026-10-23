# RepRespond 🎧

> **Less searching. More solving.**

RepRespond is a full-stack MERN application built to eliminate friction for customer service representatives. It provides a fast, searchable database of communication templates, allowing agents to instantly find and copy the exact phrasing they need while handling live customer issues.

## 🚀 Features

* **Instant Search:** Quickly find templates using keywords and tags.
* **One-Click Copy:** Copy optimized responses to the clipboard instantly without formatting issues.
* **Category Filtering:** Easily navigate between departments like Billing, Tech Support, and Sales.
* **Trending Templates:** Automatically tracks template usage to surface the most effective and popular responses.
* **Agent & Admin Roles:** Secure authentication allowing agents to view/copy and admins to create/edit templates.

## 🛠️ Tech Stack

**Frontend:**
* React (Vite)
* React Router DOM
* Native Fetch API
* CSS Grid/Flexbox (or your preferred UI framework)

**Backend:**
* Node.js
* Express.js
* MongoDB & Mongoose
* JSON Web Tokens (JWT) for authentication
* bcryptjs for password hashing

## 📦 Getting Started

Follow these steps to set up the project locally.

### Prerequisites

* Node.js installed on your machine
* A MongoDB database (local or MongoDB Atlas)

### Installation

1. **Clone the repository:**
   ```bash
   git clone https://github.com/your-username/RepRespond.git
   cd RepRespond
   ```

2. **Install Backend Dependencies:**
   ```bash
   cd server
   npm install
   ```

3. **Install Frontend Dependencies:**
   ```bash
   cd ../client
   npm install
   ```

### Environment Variables

Create a `.env` file in the `server` directory and add the following:

```env
PORT=5000
MONGO_URI=your_mongodb_connection_string
JWT_SECRET=your_super_secret_jwt_key
```

### Running the App

1. **Start the backend server:**
   ```bash
   cd server
   npm run dev
   ```

2. **Start the frontend client:**
   ```bash
   cd client
   npm run dev
   ```

## 🔌 API Endpoints

### Authentication
* `POST /api/auth/register` - Register a new user account (Agent or Admin).
* `POST /api/auth/login` - Authenticate a user and receive a JWT.

### Templates
* `GET /api/templates` - Retrieve all templates.
* `GET /api/templates/search?q=keyword` - Search for templates matching a keyword in the title, content, or tags.
* `POST /api/templates` - Create a new template (Requires Admin authorization).
* `PUT /api/templates/:id/copy` - Increment the usage count of a template when an agent copies it.

## 📂 Project Structure

* `/client` - Contains the Vite React frontend application.
* `/server` - Contains the Express backend API and MongoDB models.

## 🤝 Contributing

Contributions, issues, and feature requests are welcome! Feel free to check the issues page if you want to contribute.

## 📝 License

This project is licensed under the MIT License.