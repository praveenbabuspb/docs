# Mobile Layout: Shop Home Page (`/shop/[shopName]`)

This document describes the visual layout of an individual shop's homepage on a mobile viewport.

## 1. Screen Structure

The page is structured vertically and is fully scrollable.
1.  **Page Header:** Contains navigation and shop identification.
2.  **Main Content:** Displays the shop's categories and featured products.
3.  **Floating Cart:** Persistently visible in the bottom-right corner.

---

## 2. Component Placement

### 2.1. Page Header

-   **Location:** At the very top of the screen.
-   **Structure:**
    -   **Top Left:** A "Back" arrow icon and link that navigates the user to the previous page (the shops list for that category).
    -   **Center:** The shop's name and a short description are displayed prominently in the center.

### 2.2. Main Content Area (Scrollable)

This area appears directly below the header. Content is arranged vertically in the following order:

1.  **Promotional Carousel (`HomeCarousel`):** A full-width carousel for shop-specific banners or promotions.
2.  **Shop by Category Section (`ShopCategoryCircles`):**
    -   A section with a centered title, "Shop by Category".
    -   A horizontally-scrolling carousel containing circular category images. Each circle represents an internal category within the shop (e.g., "Grocery", "Snacks"). Tapping a category navigates to the product listing page.
3.  **Best Sellers Section (`AutoProductCarousel`):**
    -   A section titled "Best Sellers".
    -   A horizontally-scrolling, auto-playing carousel of `ProductCard` components showcasing the shop's most popular items.
4.  **New Arrivals Section (`ProductCarousel`):**
    -   A section titled "New Arrivals".
    -   A horizontally-scrolling carousel of `ProductCard` components showcasing the newest items in the shop.

### 2.3. Floating Cart (`FloatingCart`)

-   **Location:** A circular button fixed in the bottom-right corner.
-   **Functionality:** Displays the cart icon and the total number of items from all carts. Tapping it opens the mini-cart side panel.
