# Real-Time-Location-Tracker-for-Multivendor-Delivery-Platform
Implement a real-time location tracking system like Rapido or Dunzo for a multivendor marketplace. Vendors assign delivery partners to orders, and users can track the rider’s live location.
# Real-Time Location Tracking for Multi-Vendor Marketplace

This project implements a real-time location tracking system, inspired by services like Rapido or Dunzo, tailored for a multi-vendor marketplace. It allows vendors to assign delivery partners to orders, and customers to track the live location of their assigned delivery partner on a map.

The application is built with a modern full-stack architecture, leveraging Next.js for the frontend, Node.js/Express for the backend, Firestore for the database, and Socket.IO for real-time communication.

## ✨ Features

The system supports three main user roles: **Vendor**, **Delivery Partner**, and **Customer**, each with a dedicated dashboard and specific functionalities.

### 📦 Vendor Dashboard
* **Order Management:** View a comprehensive list of all their orders.
* **Delivery Partner Assignment:** Assign available delivery partners to pending orders.
* **Order Status Tracking:** See the current status of each order (pending, assigned, on the way, delivered, cancelled).
* **Order Creation (Demo):** A simplified button to create new orders for testing purposes.

### 🛵 Delivery Partner Dashboard
* **Assigned Orders:** View a list of orders currently assigned to them.
* **"Start Delivery" Action:** A button to initiate the delivery process, which automatically begins real-time location tracking.
* **Live Location Simulation:** Simulates live location updates using a predefined path, sending coordinates to the backend every 2-3 seconds via WebSockets.
* **"Complete Delivery" Action:** A button to mark an order as delivered, stopping location tracking for that order.
* **"Stop Tracking" Action:** Manually stop location updates.

### 📍 Customer Tracking Page
* **Real-time Map:** Displays a map (powered by Leaflet.js) showing the live location of the assigned delivery partner.
* **Auto-Updates:** The delivery partner's location marker on the map automatically updates every 2-3 seconds as new data is pushed from the backend via WebSockets.
* **Order Details:** View essential order information and delivery status.
* **Public Access:** Accessible via a unique tracking ID, no login required for customers to track their specific order.

## 🏛️ Architecture

The application follows a client-server architecture with real-time capabilities enabled by WebSockets.

+---------------------+         +----------------------------+         +-----------------+
|     Frontend        |         |        Backend API         |         |     Database    |
| (Next.js / React)   |         | (Node.js / Express / TS)   |         |    (Firestore)  |
|                     |         |                            |         |                 |
| - Vendor Dashboard  | <-----> | - Auth (Login/Signup)      | <-----> | - Users         |
| - Delivery Dashboard|   REST  | - Vendor APIs              |   CRUD  | - Vendors       |
| - Customer Tracking |         | - Delivery Partner APIs    |         | - Orders        |
+----------^----------+         | - Customer Tracking API    |         | - Locations (History)|
|                    +-------------^--------------+         +-----------------+
|  WebSocket (Socket.IO)           |
+----------------------------------+
Real-time Location Updates


* **Frontend (Next.js):** A server-side rendered React application providing a responsive user interface for all three roles. It communicates with the backend via REST APIs for initial data fetching and state changes, and uses Socket.IO for real-time location updates.
* **Backend (Node.js/Express):** A TypeScript-based RESTful API server. It handles user authentication (signup/login), manages orders, assigns delivery partners, and processes location updates.
* **Database (Firestore):** A NoSQL cloud database used for persistent storage of user data, vendor information, orders, and location history. Its real-time capabilities (though not directly used for live updates in this setup, but for data persistence) are well-suited for dynamic applications.
* **WebSockets (Socket.IO):** The backbone of the real-time tracking feature. Delivery partners push their location updates to the server via WebSockets, and the server then broadcasts these updates to relevant customer tracking pages.
* **Authentication (JWT):** JSON Web Tokens are used for secure authentication across all user roles, ensuring that only authorized users can access specific API endpoints.
* **Multitenancy Logic:** The backend implements logic to ensure that vendors can only view and manage their own orders, providing data isolation.

## 🚀 Tech Stack

* **Frontend:**
    * [Next.js](https://nextjs.org/) (React Framework)
    * [TypeScript](https://www.typescriptlang.org/)
    * [Tailwind CSS](https://tailwindcss.com/) (for rapid styling)
    * [Leaflet.js](https://leafletjs.com/) & [React-Leaflet](https://react-leaflet.js.org/) (for interactive maps)
    * [Socket.IO Client](https://socket.io/docs/v4/client-api/)
* **Backend:**
    * [Node.js](https://nodejs.org/)
    * [Express.js](https://expressjs.com/) (Web Framework)
    * [TypeScript](https://www.typescriptlang.org/)
    * [Firestore](https://firebase.google.com/docs/firestore) (Database)
    * [Socket.IO](https://socket.io/) (WebSockets)
    * [JWT (jsonwebtoken)](https://www.npmjs.com/package/jsonwebtoken) (Authentication)
    * [Bcrypt.js](https://www.npmjs.com/package/bcryptjs) (Password Hashing)
    * [UUID](https://www.npmjs.com/package/uuid) (for generating unique IDs)

## 🛠️ Setup Instructions

Follow these steps to get the project up and running on your local machine.

### Prerequisites

* Node.js (v18 or higher recommended)
* npm (comes with Node.js)
* A Firebase Project (for Firestore and Firebase Auth setup if running outside Canvas environment)

### 1. Backend Setup

1.  **Clone the Repository:**
    ```bash
    git clone <repository-url>
    cd real-time-location-tracker # Or whatever your cloned directory is named
    ```
2.  **Navigate to Backend Directory:**
    ```bash
    cd backend
    ```
3.  **Install Dependencies:**
    ```bash
    npm install
    ```
4.  **Environment Variables:**
    * Create a `.env` file in the `backend/` directory.
    * Copy the content from `.env.example` into your new `.env` file.
    * **Important:**
        * `PORT`: The port your backend will run on (e.g., `5000`).
        * `FRONTEND_URL`: The URL of your frontend application (e.g., `http://localhost:3000`).
        * `JWT_SECRET`: A strong, random string for JWT signing. **Change this in production!**
        * **Firebase Configuration:** If you are running this *outside* a Canvas-like environment that provides `__firebase_config` and `__app_id` globally, you **MUST** uncomment and fill in your Firebase project's configuration details in the `.env` file. You can find these in your Firebase project settings -> Project settings -> General -> Your apps -> Firebase SDK snippet (Config).

    ```env
    # backend/.env
    PORT=5000
    FRONTEND_URL=http://localhost:3000
    JWT_SECRET=your_very_secure_and_long_jwt_secret_here

    # Uncomment and fill if NOT running in a Canvas environment
    # FIREBASE_API_KEY="AIzaSy..."
    # FIREBASE_AUTH_DOMAIN="your-project.firebaseapp.com"
    # FIREBASE_PROJECT_ID="your-project-id"
    # FIREBASE_STORAGE_BUCKET="your-project.appspot.com"
    # FIREBASE_MESSAGING_SENDER_ID="1234567890"
    # FIREBASE_APP_ID="1:1234567890:web:abcdef12345"
    ```
5.  **Run the Backend:**
    * For development (with live reload):
        ```bash
        npm run dev
        ```
    * For production (build and run):
        ```bash
        npm run build
        npm start
        ```
    The backend server should now be running on `http://localhost:5000` (or your specified port).

### 2. Frontend Setup

1.  **Navigate to Frontend Directory:**
    ```bash
    cd ../frontend
    ```
2.  **Install Dependencies:**
    ```bash
    npm install
    ```
3.  **Environment Variables:**
    * Create a `.env.local` file in the `frontend/` directory.
    * Copy the content from `.env.local.example` into your new `.env.local` file.
    * Ensure `NEXT_PUBLIC_BACKEND_URL` matches the port your backend is running on.

    ```env
    # frontend/.env.local
    NEXT_PUBLIC_BACKEND_URL=http://localhost:5000
    ```
4.  **Run the Frontend:**
    ```bash
    npm run dev
    ```
    The frontend application should now be accessible at `http://localhost:3000` (or your specified port).

## 🚀 Usage Guide

Once both the backend and frontend are running:

1.  **Access the Frontend:** Open your web browser and go to `http://localhost:3000`.
2.  **Sign Up:**
    * Click on "Sign Up".
    * Create accounts for each role:
        * **Vendor:** Choose `Vendor` role, provide `Vendor Name`, Email, and Password.
        * **Delivery Partner:** Choose `Delivery Partner` role, Email, and Password.
        * **Customer:** Choose `Customer` role, Email, and Password.
3.  **Login as Vendor:**
    * Login with your Vendor account.
    * You will be redirected to the Vendor Dashboard.
    * **Create Orders (for demo):** Click the "Create New Order (For Demo)" button. You will be prompted to enter a Customer User ID. Use the `id` of a customer account you created (you can find this ID in your backend logs when the customer signs up, or you can retrieve it from the Firestore `users` collection). This will create a new order visible on your dashboard. Note the `Tracking ID` generated for this order.
4.  **Login as Delivery Partner:**
    * Login with your Delivery Partner account.
    * You will see orders assigned to you (if any).
    * **Start Delivery:** For an order with `status: assigned`, click "Start Delivery". This will update the order status to "on_the_way" and begin simulating real-time location updates from the delivery partner.
    * **Complete Delivery:** Once the delivery is simulated, click "Complete Delivery" to mark the order as delivered and stop tracking.
5.  **Track as Customer:**
    * You don't need to be logged in as a customer to track.
    * Use the `Tracking ID` obtained from the Vendor Dashboard.
    * Navigate directly to the tracking page: `http://localhost:3000/track/YOUR_TRACKING_ID`.
    * You should see the delivery partner's live location updating on the map if they have started the delivery.

## 📁 Project Structure

The project is organized into two main directories: `backend` and `frontend`.

.
├── backend/                  # Node.js, Express, TypeScript backend
│   ├── src/                  # Source code for the backend
│   │   ├── config/           # Firebase configuration
│   │   ├── middleware/       # Authentication middleware
│   │   ├── models/           # Conceptual data models for Firestore
│   │   ├── routes/           # API route definitions
│   │   ├── utils/            # Utility functions (JWT)
│   │   ├── types.ts          # Shared TypeScript types
│   │   └── server.ts         # Main Express server and Socket.IO setup
│   ├── .env.example          # Example environment variables
│   ├── package.json          # Backend dependencies and scripts
│   ├── tsconfig.json         # TypeScript configuration for backend
│   └── README.md             # Backend-specific README (optional)
├── frontend/                 # Next.js, React, TypeScript frontend
│   ├── public/               # Static assets
│   ├── src/                  # Source code for the frontend
│   │   ├── components/       # Reusable React components (e.g., Layout)
│   │   ├── context/          # React Context for authentication
│   │   ├── pages/            # Next.js pages (routes)
│   │   │   ├── _app.tsx
│   │   │   ├── index.tsx
│   │   │   ├── login.tsx
│   │   │   ├── signup.tsx
│   │   │   ├── vendor.tsx
│   │   │   ├── delivery.tsx
│   │   │   ├── customer.tsx
│   │   │   └── track/[trackingId].tsx
│   │   ├── styles/           # Global CSS and Tailwind directives
│   │   ├── types/            # Shared TypeScript types
│   │   └── config/           # Frontend configuration (e.g., backend URL)
│   ├── .env.local.example    # Example local environment variables
│   ├── next.config.js        # Next.js configuration
│   ├── package.json          # Frontend dependencies and scripts
│   ├── postcss.config.js     # PostCSS configuration for Tailwind
│   ├── tailwind.config.ts    # Tailwind CSS configuration
│   └── tsconfig.json         # TypeScript configuration for frontend
└── .gitignore                # Git ignore file
└── README.md                 # Main project README (this file)


## 💎 Clean TypeScript Typing

Throughout the project, a strong emphasis has been placed on using TypeScript to ensure type safety and improve code maintainability.
* **Shared Types:** Common interfaces for `User`, `Order`, `LocationUpdate`, etc., are defined in `types.ts` files in both `backend/src` and `frontend/src/types` to ensure consistency.
* **Express Request Augmentation:** The `Express.Request` interface is extended in `backend/src/types.ts` to include `db`, `auth`, `currentUserId`, and `user` properties, providing clear types for data injected by middleware.
* **Component Props and State:** React components and hooks are fully typed, ensuring that props and state variables adhere to defined interfaces.
* **API Responses:** Expected API response structures are implicitly handled by the types, ensuring correct data access.

## ☁️ Deployment

While not mandatory for the submission, this project is designed for easy deployment:

* **Frontend (Next.js):** Can be deployed effortlessly to platforms like [Vercel](https://vercel.com/) (the creators of Next.js). Simply connect your GitHub repository, and Vercel will automatically detect and deploy the Next.js application.
* **Backend (Node.js/Express):** Can be deployed to services like [Render](https://render.com/) or [Railway](https://railway.app/). These platforms support Node.js applications and can connect to external MongoDB/Firestore services. You would need to set up your environment variables (especially `JWT_SECRET` and Firebase credentials) on the chosen hosting platform.

**Deployment Link:** (Placeholder - insert your live deployment URL here if available)

## 🔄 Git Commits Showing Development Process

To demonstrate a logical development process, consider the following commit structure when building out a project like this:

1.  `feat: Initialize backend with Express, TS, and basic structure`
2.  `feat: Setup Firestore connection and auth middleware`
3.  `feat: Implement user signup and login APIs (bcrypt, JWT)`
4.  `feat: Initialize frontend with Next.js, TS, and TailwindCSS`
5.  `feat: Implement AuthContext for frontend authentication`
6.  `feat: Create login and signup pages in frontend`
7.  `feat: Setup Socket.IO on backend for real-time communication`
8.  `feat: Implement vendor dashboard API for orders and DPs`
9.  `feat: Develop vendor dashboard UI to view/assign orders`
10. `feat: Implement delivery partner dashboard API for assigned orders`
11. `feat: Develop delivery partner dashboard UI with start/complete delivery`
12. `feat: Implement location update API and Socket.IO emission from DP`
13. `feat: Add customer tracking API to fetch order and DP location`
14. `feat: Create customer tracking page with Leaflet map`
15. `feat: Integrate Socket.IO client for real-time map updates`
16. `refactor: Centralize shared types for frontend and backend`
17. `fix: Address minor UI/UX issues and error handling`
18. `docs: Update README with architecture, setup, and features`
19. `chore: Add .gitignore and refine package.json scripts`
