# Mobile Layout: Home Page (`/`)

This document describes the visual layout of the application's homepage on a mobile viewport.

## 1. Screen Structure

The homepage is composed of three main sections:
1.  **Header (Sticky):** Contains primary navigation and search.
2.  **Main Content (Scrollable):** The body of the page containing all promotional and product content.
3.  **Bottom Navigation Bar (Fixed):** Provides persistent navigation across the app.

---

## 2. Component Placement

### 2.1. Header

-   **Location:** Top of the screen, remains visible on scroll.
-   **Structure (Top Row):**
    -   **Left:** Hamburger Menu icon button to open the side navigation drawer.
    -   **Center:** Location Selector component, displaying the current delivery area.
    -   **Right:** Profile icon button. If the user is logged in, it shows their avatar; otherwise, it's a generic user icon that links to the login page.
-   **Structure (Middle Row):**
    -   The "Super Saver" toggle switch is displayed directly below the top header row.
-   **Structure (Bottom Row):**
    -   A full-width Search Bar is located at the bottom of the header section. It displays placeholder text like "Search..." and opens a search dialog when tapped.

### 2.2. Main Content Area (Scrollable)

This area appears directly below the header. Content is arranged vertically in the following order:

1.  **Primary Carousel (`HomeCarousel`):** A full-width, auto-playing carousel displaying large promotional banners.
2.  **Deals Section (`DealsSection`):** A section displaying two large "Deal Cards" side-by-side or stacked vertically, each with its own internal carousel of featured items.
3.  **Promo Section (`PromoSection`):** A single, full-width card that contains:
    -   A main promotional image and title on the left.
    -   A grid of smaller, circular category images on the right.
4.  **Shop Sections (`ShopSection`):**
    -   Multiple `ShopSection` components are rendered sequentially down the page.
    -   Each section is dedicated to a specific shop (e.g., "FreshMart Supermarket").
    -   **Header:** Displays the shop's name on the left and a "See All" link on the right.
    -   **Content:** Contains one or more horizontally-scrolling carousels (`ProductCarousel`) of product cards from that shop.

### 2.3. Bottom Navigation (`NavBar`)

-   **Location:** Fixed to the bottom of the screen, floating above the content.
-   **Appearance:** A rounded, pill-shaped container centered horizontally.
-   **Content:** Contains a series of icon-only buttons for primary navigation links like "Home," "Orders," "About," and "Contact." The active page's icon has an illuminated "lamp" indicator behind it.

### 2.4. Floating Action Buttons

-   **Floating Cart (`FloatingCart`):** A circular button fixed in the bottom-right corner, displaying the cart icon and item count. It sits above the `NavBar`.
-   **Floating Track Order Button (`FloatingTrackOrderButton`):** If there is an active order, this circular button appears in the bottom-left corner, displaying a truck icon and the order's current status.