
# Product Requirements Document: Q-Commerce Central Backend

**Version:** 1.0
**Date:** 2024-07-25
**Author:** Firebase Studio AI

---

## 1. Introduction

### 1.1. Project Overview
This document outlines the product requirements for the backend system of the **Q-Commerce Central** application. This backend serves as the single, unified API and data management layer for the entire Q-Commerce ecosystem, which includes the Super Admin Dashboard, consumer-facing mobile/web applications, and other future interfaces.

The backend is built using a modern, serverless architecture with Next.js API Routes and is deployed via Firebase App Hosting. It leverages Firebase services (Firestore, Authentication) for robust data storage and security.

### 1.2. Purpose
The primary purpose of this backend is to provide a secure, scalable, and centralized system to manage all data and business logic for the Q-Commerce platform. It standardizes how data is created, retrieved, updated, and deleted, ensuring consistency and security across all frontend applications.

### 1.3. Goals
- **Centralize Business Logic:** Create a single source of truth for all business operations.
- **Ensure Security:** Protect data and actions through robust authentication and role-based authorization.
- **Enable Scalability:** Utilize a serverless architecture that scales automatically with demand.
- **Support Multiple Frontends:** Provide a consistent API for the Super Admin Dashboard, consumer apps, and future clients.
- **Facilitate Real-time Features:** Lay the groundwork for real-time capabilities like order tracking.

---

## 2. User Roles & Permissions

The system defines several user roles with specific permissions. Access to API endpoints is strictly controlled based on these roles.

| Role                 | Description                                                                                               | Key Permissions                                                                                                                                  |
| -------------------- | --------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------ |
| **`super_admin`**    | The system owner with full access to all data and settings across the entire platform.                      | Can create other admins and stores. Can view all users, orders, and system-wide analytics. Can manage the entire application.                    |
| **`store_admin`**    | (Future Role) An administrator responsible for managing one or more specific stores.                        | Can manage their assigned store's inventory, orders, and staff. Cannot view data from other stores.                                            |
| **`delivery_executive`**| A user responsible for picking up and delivering orders.                                                   | Can view and accept assigned orders. Can update order status (e.g., "Out for Delivery," "Delivered"). Can update their real-time location.     |
| **`customer`**       | (Future Role) An end-user who browses products and places orders through the consumer application.          | Can manage their own profile and addresses. Can place orders and view their own order history. Can track their own orders.                     |

---

## 3. Functional Requirements & Features

This section details the backend's core features, grouped by domain.

### 3.1. Authentication
- **User Registration:**
  - The first user to ever register becomes the `super_admin`.
  - The registration API is then protected; subsequent super admins can only be created by an existing super admin.
- **User Login:** Standard email/password login is handled by the Firebase client SDK. The backend verifies the user's ID token on every authenticated request.
- **Role-Based Access Control (RBAC):** A middleware (`withAuth`) protects API routes, verifying the user's token and checking if their role grants them permission to perform the requested action.

### 3.2. User Management
- The system can create, retrieve, update, and delete users.
- The `super_admin` can list all users in the system and manage their profiles and roles.
- Users can manage their own profile information.

### 3.3. Address Management
- The system allows for storing multiple delivery addresses per user.
- Each address includes a label (e.g., "Home," "Work"), the full address string, and precise geo-coordinates (`latitude`, `longitude`).
- Users can add, update, and delete their own saved addresses.

### 3.4. Order Management
- The system can create, retrieve, update, and delete orders.
- Customers can place orders, which include items, a selected delivery address, payment mode, and total cost.
- Admins can view and manage all orders in the system.
- Order status can be updated (e.g., 'Pending', 'Shipped', 'Fulfilled', 'Declined').

### 3.5. Real-time Order Tracking
- The backend provides an endpoint to retrieve the last known location of a delivery executive for a specific order.
- _Note: The current implementation provides mock data. A future implementation will integrate with Firebase Realtime Database to stream live coordinates from the delivery executive's device._

### 3.6. ETA & Distance Calculation
- The backend provides an endpoint to estimate the travel time and distance between two geographical points.
- _Note: The current implementation provides a mock response. This is a placeholder for a future integration with a service like the Google Maps Distance Matrix API._

### 3.7. Store Management
- The `super_admin` can create, update, and delete stores.
- Each store has a name, location, type (e.g., 'Grocery', 'Pharmacy'), and status ('Open'/'Closed').
- The system supports managing store-specific data, including product catalogs, categories, and staff assignments (admins, executives).
- Data is structured to support a multi-store architecture efficiently.

---

## 4. Technical Architecture

- **Framework:** Next.js (App Router)
- **Deployment:** Firebase App Hosting (Serverless via Cloud Run)
- **Database:** Cloud Firestore for primary data storage (Users, Orders, Stores, etc.). Firebase Realtime Database is planned for live tracking.
- **Authentication:** Firebase Authentication
- **AI/ML:** Genkit for generative AI features within the AI Studio.

The backend logic resides in the `src/app/api/` directory. Next.js file-based routing automatically maps these files to API endpoints. The entire application is deployed as a single, cohesive serverless service, simplifying management and ensuring scalability.

---

## 5. API Endpoint Summary

This is a high-level summary. For detailed request/response examples, refer to `API_ENDPOINTS.md` and `CONSUMER_API_ENDPOINTS.md`.

- **Authentication:** `POST /api/auth/register`, `POST /api/auth/login`
- **Users:** `GET /api/users`, `POST /api/users`, `GET, PUT, DELETE /api/users/{userId}`
- **Addresses:** `GET, POST /api/users/{userId}/addresses`, `PUT, DELETE /api/users/{userId}/addresses/{addressId}`
- **Orders:** `GET, POST /api/orders`, `GET, PUT, DELETE /api/orders/{orderId}`
- **Tracking:** `GET /api/orders/{orderId}/tracking`
- **Distance:** `GET /api/distance`
- **Stores:** `GET, POST /api/stores`, `GET, PUT, DELETE /api/stores/{storeId}`
- **Store Categories:** `GET, POST /api/stores/{storeId}/categories`, `PUT, DELETE /api/stores/{storeId}/categories/{categoryId}`
- **Delivery Executives:** `GET, POST /api/delivery-executives`, `GET, PUT, DELETE /api/delivery-executives/{executiveId}`

