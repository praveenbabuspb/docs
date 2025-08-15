# Frontend Architecture: React Nav

**Version:** 1.0
**Date:** 2023-10-27

---

## 1. Introduction & Philosophy

This document provides a comprehensive architectural overview of the React Nav frontend. It is intended to be the central source of truth for understanding the structure, data flow, and functionality of the application.

The frontend is built on a modern, robust technology stack:

-   **Framework:** Next.js 14+ with the App Router
-   **Language:** TypeScript
-   **UI Library:** React
-   **Component Library:** shadcn/ui
-   **Styling:** Tailwind CSS
-   **State Management:** Zustand
-   **Backend Communication:** Asynchronous requests via the native `fetch` API to Next.js API Routes.

The core philosophy is to build a scalable, maintainable, and performant e-commerce platform by leveraging server components, creating reusable UI blocks, and maintaining a clear separation of concerns between state, services, and UI.

---

## 2. Root Directory Structure

-   **`public/`**: Stores static assets accessible from the browser, most notably the `firebase-messaging-sw.js` for push notifications.
-   **`src/`**: Contains all the application source code.
    -   **`app/`**: The core of the Next.js application, following the App Router paradigm.
    -   **`components/`**: All reusable React components.
    -   **`data/`**: Mock data services that simulate a backend.
    -   **`hooks/`**: Custom React hooks for shared logic.
    -   **`lib/`**: Utility functions and library initializations.
    -   **`services/`**: The data abstraction layer.
    -   **`store/`**: Global state management using Zustand.

---

## 3. `src/app/` - Application Pages & Routes

This directory uses the Next.js App Router. Each folder represents a URL segment.

### 3.1. Root Files

-   **`layout.tsx`**: The root layout for the entire application. It imports global CSS, sets up the HTML shell, loads the 'Inter' font, and includes the `<Toaster />` component for notifications.
-   **`globals.css`**: The global stylesheet. It contains Tailwind CSS base layers and defines all the HSL CSS variables that constitute the application's theme (colors, border-radius) used by shadcn/ui.
-   **`error.tsx`**: A global error boundary. If any part of the application throws an unhandled error, this component will be rendered, providing a user-friendly error message and an option to retry the action.
-   **`page.tsx` (`/`)**: The **Homepage**.
    -   **Functionality:** This is the main landing page. It fetches and displays main navigation categories, featured shops, and a selection of their products. It adapts its content based on the "Super Saver" toggle.
    -   **Data Flow:** Fetches categories, shops, and products from `DataService` on the client side. The loading state is handled by showing `Skeleton` components.
    -   **User Flow:** Users can browse deals, promotional banners, and product carousels. They can navigate to shop pages or category listing pages from here.
    -   **Components:** `MainLayout`, `HomeCarousel`, `DealsSection`, `PromoSection`, `ShopSection`.

### 3.2. Page-Specific Routes

-   **`/about/page.tsx`**: A static informational page.
-   **`/admin/page.tsx`**: **Admin Dashboard**.
    -   **Functionality:** Allows authenticated admin users to view all orders placed on the platform. It provides an interface to update the status of each order.
    -   **Data Flow:** Fetches the list of all orders from the `/api/admin/orders` endpoint. When an admin changes an order's status, it sends a `PUT` request to `/api/admin/orders/[orderId]`. The change triggers the `onOrderStatusUpdate` Cloud Function, which sends a push notification to the customer.
    -   **User Flow:** An admin logs in, navigates to this page, views orders, and updates their status via a dropdown. A "Refresh Orders" button allows them to fetch the latest data.
-   **`/backend-test/page.tsx`**: A utility page for developers to test API endpoints.
-   **`/checkout/page.tsx`**: **Checkout Page**.
    -   **Functionality:** Guides the user through the final steps of placing an order. It handles single-shop checkouts and a "global" checkout for all items in the cart.
    -   **Data Flow:** Reads cart information from the `cartStore`. It reads user address information from the `authStore`. When the user places an order, it sends all relevant data (items, total, shipping address) to the `/api/orders` endpoint.
    -   **User Flow:** User reviews items, selects a delivery address (or adds a new one), adds a tip, and confirms payment (currently Cash on Delivery). Upon success, they are redirected to the order tracking page.
    -   **Components:** `AddressPicker`, `DeliveryTipSelector`, and various `Dialog` and `Card` components from shadcn.
-   **`/login/page.tsx`**: **User Login Page**.
    -   **Functionality:** Provides a form for users to log in.
    -   **Data Flow:** Uses the `login` function from the `authStore`, which communicates with Firebase Authentication.
    -   **User Flow:** User enters email and password. On success, they are redirected to the homepage. On failure, an error message is shown.
-   **`/register/page.tsx`**: **User Registration Page**.
    -   **Functionality:** Provides a form for new users to create an account.
    -   **Data Flow:** Uses the `register` function from the `authStore`, which sends a request to the `/api/auth/register` endpoint to create the user in Firebase.
    -   **User Flow:** User fills in their details. On success, they are automatically logged in and redirected to the homepage.
-   **`/orders/page.tsx`**: **My Orders Page**.
    -   **Functionality:** Displays a list of the currently logged-in user's past and present orders.
    -   **Data Flow:** Reads the `orders` array from the `user` object in the `authStore`.
    -   **User Flow:** User can see a summary of each order (ID, date, total, status) and click to track it.
-   **`/track-order/page.tsx`**: **Order Tracking Page**.
    -   **Functionality:** Shows a detailed view of a single order's status and journey.
    -   **Data Flow:** Retrieves the specific order details from the `authStore` based on the `orderId` in the URL query parameters.
    -   **User Flow:** User sees a map placeholder, a progress bar indicating the order status (e.g., Confirmed -> Shipped -> Delivered), and a summary of the items in the order.
-   **`/profile/**`**: The user account management section.
    -   **`/profile/page.tsx`**: The main profile hub, with navigation tiles to other settings pages.
    -   **`/profile/addresses/page.tsx`**: Allows users to view, add, edit, and delete their saved delivery addresses. Uses the `AddressPicker` component and communicates with the `/api/user/addresses` endpoints.
    -   **`/profile/notifications/page.tsx`**: Allows users to enable push notifications for order updates. It interacts with the browser's Notification API and the `initFcm` function from the `authStore`.
    -   Other pages (`information`, `payments`, `rewards`, `wallet`) are placeholders for future development.
-   **`/shops/[category]/page.tsx`**: **Shops by Category Page**.
    -   **Functionality:** A dynamic page that lists all shops belonging to a specific main category (e.g., "Supermarket").
    -   **Data Flow:** Extracts the `category` name from the URL, fetches the list of corresponding shops from `DataService`.
    -   **User Flow:** User clicks a category on the homepage and lands here. They see a list of shop cards and can click on one to navigate to that shop's specific page.
-   **`/shop/[shopName]/page.tsx`**: **Shop Home Page**.
    -   **Functionality:** A dedicated landing page for an individual shop.
    -   **Data Flow:** Fetches the shop's details, its internal categories, and featured products from `DataService`.
    -   **User Flow:** User browses shop-specific promotions, categories (via `ShopCategoryCircles`), and product carousels.
-   **`/shop/[shopName]/[categoryName]/page.tsx`**: **Product Listing Page**.
    -   **Functionality:** Displays all products within a specific category of a specific shop, using a two-pane layout.
    -   **Data Flow:** Fetches all products for the given shop and category from `DataService`. It then creates a unique list of sub-categories from that product data.
    -   **User Flow:** User navigates using the vertical sub-category list on the left. The product grid on the right updates to show items from the selected sub-category. Users can add items to the cart directly from this page.

---

## 4. `src/components/` - Reusable Components

This directory is divided into application-specific components and the base UI library.

### 4.1. `ui/` Directory
This contains all the standard, un-styled components provided by `shadcn/ui`, such as `Button`, `Card`, `Dialog`, `Input`, etc. They are the fundamental building blocks of the application's design system, styled by `globals.css` and Tailwind CSS.

### 4.2. Application-Specific Components

-   **`header.tsx`**: The main site header. On desktop, it's a full bar with navigation and search. On mobile, it's a more compact version with a hamburger menu and search input. It also handles showing the user's avatar/login button.
-   **`navbar.tsx`**: The fixed bottom navigation bar on mobile, providing quick access to primary app sections like Home and Orders. It features a "lamp" effect to indicate the active page.
-   **`main-layout.tsx`**: A wrapper component that combines the `Header`, `NavBar`, `FloatingCart`, and `FloatingTrackOrderButton`. It ensures a consistent layout across most pages.
-   **`floating-cart.tsx`**: The persistent cart button in the bottom-right corner. It displays the total item count and opens a side panel (mini-cart) showing a breakdown of carts by shop.
-   **`floating-track-order-button.tsx`**: A persistent button in the bottom-left that appears only when there is an active order being processed. It provides a quick link to the order tracking page.
-   **`product-card.tsx`**: The standard card used to display a single product in grids and carousels. It shows the product image, name, price, and an "Add to Cart" button that transforms into a quantity stepper.
-   **`home-carousel.tsx`, `product-carousel.tsx`, `auto-product-carousel.tsx`**: Wrapper components around the `Carousel` from shadcn, configured for different use cases (e.g., banner display, manual product scrolling, auto-playing product scrolling).
-   **`shop-section.tsx`**: A component used on the homepage to display a single shop's name and several carousels of its products, grouped by category.
-   **`shop-category-circles.tsx`**: A horizontally scrolling list of circular images used on a shop's page to represent its internal categories.
-   **`address-picker.tsx`**: A component used within a dialog to add or edit a delivery address. It includes fields for the address details and a button to fetch GPS coordinates.
-   **`delivery-tip-selector.tsx`**: A component used on the checkout page that provides preset buttons and a custom input field for adding a delivery tip.
-   **`search-dialog.tsx`**: A responsive dialog/sheet for search functionality. It shows recent and popular searches as suggestions.
-   **`super-saver-toggle.tsx`**: The toggle switch in the header that allows users to switch between the regular and "Super Saver" modes of the app.

---

## 5. `src/lib/`, `src/hooks/`, `src/services/`, `src/store/`

### 5.1. `lib/`
-   **`utils.ts`**: Contains the `cn` utility function from `tailwind-merge` for intelligently combining CSS classes.
-   **`iconMapper.ts`**: A mapping utility that takes a string name (e.g., "ShoppingCart") and returns the corresponding icon component from `lucide-react`. This allows for dynamic icon rendering from data.
-   **`firebase.ts`**: Handles the **client-side** initialization of the Firebase SDK.
-   **`firebase-messaging.ts`**: Contains the client-side logic (`getFcmToken`) for requesting notification permissions and retrieving the device's FCM token.

### 5.2. `hooks/`
-   **`use-mobile.tsx`**: A client-side hook that returns `true` if the viewport width is below a certain breakpoint, allowing components to adapt their rendering for mobile devices.
-   **`use-toast.ts`**: The custom hook that powers the application's notification (toast) system.

### 5.3. `services/`
-   **`dataService.ts`**: The data abstraction layer. This class contains static methods that simulate fetching data from a backend (e.g., `getProducts`, `getShopsByCategory`). It contains `setTimeout` calls to mimic network latency. In a real application, this service would be replaced with one that makes actual `fetch` calls to a live API.

### 5.4. `store/`
This directory uses **Zustand** for global, client-side state management.
-   **`cartStore.ts`**: Manages the state of all shopping carts. It handles multiple carts (one per shop) and provides actions to add items, update quantities, and clear carts. It also controls the open/closed state of the mini-cart side panel.
-   **`authStore.ts`**: Manages user authentication state. It handles login, logout, and registration. It also stores the logged-in user's profile information, including their saved addresses and order history. It listens for real-time updates from Firebase to keep user data fresh.
-   **`superSaverStore.ts`**: A simple store that holds the boolean state of the "Super Saver" toggle.
