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
