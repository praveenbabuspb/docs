# Mobile Layout: Profile Page (`/profile`)

This document describes the visual layout of the main user profile hub on a mobile viewport.

## 1. Screen Structure

The page is a single, scrollable view containing a series of cards with user information and navigation links.

---

## 2. Component Placement (Vertical Order)

1.  **User Info Card:**
    -   **Location:** At the top of the page.
    -   **Structure:**
        -   **Left:** A large `Avatar` component displaying the user's initials.
        -   **Right:** The user's full name, email address, and phone number.

2.  **Quick Actions Grid:**
    -   **Location:** Directly below the User Info Card.
    -   **Structure:** A grid of three `QuickActionTile` components, arranged horizontally. Each tile has an icon and a label for key actions like "My Orders," "Help Center," and "Wallet."

3.  **Primary Settings List Card:**
    -   **Location:** Below the quick actions.
    -   **Structure:** A card containing a vertical list of `SettingsItem` components. Each item includes an icon, a label (e.g., "Profile Information," "Saved Addresses"), and a chevron icon on the right, indicating it's a navigation link.

4.  **Other Info List Card:**
    -   **Location:** Below the primary settings.
    -   **Structure:** Similar to the above card, but for secondary links like "Notifications" and "About Us."

5.  **Logout Card:**
    -   **Location:** At the very bottom of the page.
    -   **Structure:** A card containing a single "Logout" button, styled to indicate a destructive action. It includes a logout icon and text.
