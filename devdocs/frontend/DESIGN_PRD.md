# Design Product Requirements Document (PRD): React Nav

**Version:** 1.0
**Date:** 2023-10-27
**Author:** App Prototyper AI

---

## 1. Introduction

### 1.1. Purpose
This document specifies the comprehensive design system for the **React Nav** e-commerce application. It serves as the official style guide to ensure visual and experiential consistency between the existing web platform and future native mobile applications. Its purpose is to provide designers and developers with a clear and definitive reference for all UI elements.

### 1.2. Core Design Philosophy
The application's design is guided by the following principles:
- **Clean & Modern:** A minimalist aesthetic that prioritizes content and removes clutter.
- **Intuitive:** A user experience that feels natural and requires minimal learning.
- **Responsive & Accessible:** A layout that adapts seamlessly to all screen sizes, from mobile to desktop, while ensuring usability for all users.
- **Trustworthy:** A professional and polished appearance that builds user confidence.

---

## 2. Brand & Visual Identity

### 2.1. Color Palette
The application uses a refined and modern color palette defined by HSL CSS variables. This palette should be strictly adhered to in native development.

| Role                  | HSL Value           | Hex (Approx.) | Description                                     |
| --------------------- | ------------------- | ------------- | ----------------------------------------------- |
| **Background**        | `0 0% 94%`          | `#F0F0F0`       | The primary page background color.              |
| **Foreground**        | `222.2 84% 4.9%`    | `#0A0A0A`       | The primary text color.                         |
| **Primary**           | `0 0% 9%`           | `#171717`       | Main interactive elements (buttons, links).     |
| **Primary Foreground**| `0 0% 98%`          | `#FAFAFA`       | Text color on Primary backgrounds.              |
| **Accent**            | `120 73% 75%`       | `#99E699`       | Highlight color for secondary elements.         |
| **Destructive**       | `0 84.2% 60.2%`     | `#F44336`       | Colors for error messages and delete actions.   |
| **Card / Popover**    | `0 0% 100%`         | `#FFFFFF`       | Background color for cards and popovers.        |
| **Border / Input**    | `0 0% 89.8%`        | `#E5E5E5`       | Default border and input field background.      |

### 2.2. Typography
A consistent typographic hierarchy is crucial for readability and visual organization.
- **Primary Font Family:** `Inter`
- **Usage:**
  - **Headlines (`font-headline`):** Use `Inter` for all headings and major titles for a clean, modern look.
  - **Body Text (`font-body`):** Use `Inter` for all paragraph text, labels, and descriptions to ensure excellent readability.

### 2.3. Iconography
- **Icon Library:** `lucide-react`
- **Style:** Icons should be used in their default line-art style. They must be simple, recognizable, and used consistently for the same actions across the app (e.g., `ShoppingCart` for the cart, `User` for the profile).
- **Size:** Icons within buttons or navigation should typically be `16px` or `20px` to maintain visual balance.

---

## 3. Component & Layout Design

### 3.1. Component Styling
All components, primarily from the `shadcn/ui` library, should follow these styling rules:
- **Borders & Corners:** Use a standard border-radius of `0.5rem` (`--radius: 0.5rem;`) for most components like buttons, cards, and inputs to create a soft, modern feel.
- **Shadows:** Apply subtle shadows (`shadow-sm`, `shadow-md`, `shadow-lg`) to cards and interactive elements on hover to create a sense of depth and elevation.
- **Interactivity:** All interactive elements (buttons, links, list items) must have clear `hover` and `focus` states, typically by changing the background color to a `muted` or `accent` shade.

### 3.2. Layout Principles
- **Responsiveness:** The layout must be fully responsive.
  - **Mobile:** Prioritize a single-column layout. Utilize a fixed **Bottom Navigation Bar** for primary app sections. The **Floating Cart** button should be persistently visible in the bottom-right corner.
  - **Desktop:** Transition to a more expansive layout. The primary navigation moves to a persistent **Top Header**.
- **Spacing:** Use Tailwind CSS spacing utilities (`p-4`, `m-6`, `space-y-4`) to maintain consistent and generous whitespace, which improves readability and reduces cognitive load.
- **Visual Hierarchy:** Use font size, weight, and color to clearly distinguish between headings, subheadings, and body text. Important actions should be styled with the `primary` color to draw user attention.

---

## 4. Specific Component Guidelines

- **Cards (`<Card>`):** The primary container for most content. They should have a white background (`--card`), a subtle border (`--border`), and a `shadow-sm`.
- **Buttons (`<Button>`):**
  - **Primary:** Solid `primary` background with `primary-foreground` text for main calls-to-action.
  - **Secondary / Outline:** Transparent background with a `border` for secondary actions.
- **Carousels (`<Carousel>`):** Used for showcasing promotional banners and product lists. They must be swipeable on mobile and include next/previous buttons on desktop.
- **Inputs & Forms:** Should have a clean appearance with a default `background` and `border`, with a visible `ring` effect on focus to indicate the active field.
- **Modals & Dialogs (`<Dialog>`):** Should appear centered with a dark overlay to focus the user's attention on the modal's content.
