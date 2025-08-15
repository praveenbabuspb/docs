# Mobile Layout: Checkout Page (`/checkout`)

This document describes the visual layout of the checkout process on a mobile viewport.

## 1. Screen Structure

The checkout page is composed of three main sections:
1.  **Header (Sticky):** A simple header with a title and a close button.
2.  **Main Content (Scrollable):** A long, scrolling list of all checkout-related options and summaries.
3.  **Footer (Fixed):** A sticky footer containing the final call-to-action button.

---

## 2. Component Placement

### 2.1. Header

-   **Location:** Sticky at the top of the screen.
-   **Structure:**
    -   **Left:** The title "Checkout".
    -   **Right:** An "X" icon button to close the checkout page and return to the previous screen.

### 2.2. Main Content Area (Scrollable)

This area contains a series of cards, arranged vertically.

1.  **Delivery Summary Card:**
    -   Displays the estimated delivery time and the number of items in the shipment.
    -   Lists each item being purchased, showing its image, name, price, and a quantity stepper (+/- buttons).
2.  **Coupons & Offers Card:**
    -   A collapsible section that allows users to view and apply available coupons.
3.  **Gifting Card:**
    -   A collapsible section that, when expanded, provides fields for a recipient's name and a gift message.
4.  **Delivery Options Card:**
    -   Contains the `DeliveryTipSelector` component for adding a tip for the delivery partner.
    -   Includes a text area for leaving notes for the delivery partner.
5.  **Delivery Address Card:**
    -   Displays a list of the user's saved addresses as radio buttons for selection.
    -   Includes an "Add a new address" button which opens a dialog (`AddressPicker`).
6.  **Bill Details Card:**
    -   A summary of the costs, including item total, discounts, delivery fees, and tips.
    -   Displays the final "To Pay" grand total.

### 2.3. Footer

-   **Location:** Fixed to the bottom of the screen.
-   **Structure:**
    -   A single, full-width button labeled "Proceed to Pay," which also displays the grand total.
-   **Functionality:** Tapping this button opens a final confirmation dialog. This button is disabled until a delivery address is selected.
