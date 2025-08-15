# Mobile Layout: Product Listing Page (`/shop/[shopName]/[categoryName]`)

This document describes the visual layout for the page where users browse products from a specific category within a shop.

## 1. Screen Structure

The page has a unique two-pane layout that is fixed and does not scroll as a whole.
1.  **Page Header:** Sits at the top, outside the two panes.
2.  **Main View (Fixed Height):** A container that holds the two panes and fills the rest of the screen height.
    -   **Left Pane (Scrollable):** Vertical navigation for sub-categories.
    -   **Right Pane (Scrollable):** A grid of products from the selected sub-category.
3.  **Floating Cart:** Persistently visible in the bottom-right corner.

---

## 2. Component Placement

### 2.1. Page Header

-   **Location:** At the very top of the screen.
-   **Structure:**
    -   **Top Left:** A "Back" arrow icon and link that navigates the user to the parent shop's page.
    -   **Below Back Link:** The main page title, which is the name of the category being viewed (e.g., "Grocery").

### 2.2. Main View

This container takes up the remaining vertical space on the screen and does not scroll.

-   **Left Pane (`CategoryNavigation`):**
    -   **Location:** A narrow vertical strip on the left side of the container.
    -   **Functionality:** This area is independently scrollable. It contains a list of sub-categories (e.g., "Grains", "Dairy", "Snacks"). Each sub-category is represented by a small image and its name. Tapping a sub-category updates the content in the right pane. The active sub-category has a visual indicator (a colored bar).

-   **Right Pane (`ProductGrid`):**
    -   **Location:** The larger area on the right side of the container.
    -   **Functionality:** This area is also independently scrollable. It displays a grid of `ProductCard` components. The grid typically shows two cards per row on a mobile screen. The products shown correspond to the sub-category selected in the left pane.

### 2.3. Floating Cart (`FloatingCart`)

-   **Location:** A circular button fixed in the bottom-right corner, overlaying the right pane.
-   **Functionality:** Displays the cart icon and the total number of items from all carts. Tapping it opens the mini-cart side panel.
