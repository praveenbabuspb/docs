# Architecture Guide: React Nav on React Native + Expo

**Version:** 1.0
**Date:** 2023-10-28

---

## 1. Introduction

This document provides a comprehensive architectural blueprint for building the **React Nav** application using React Native with the Expo managed workflow and **Expo Router** for navigation. It adapts the successful patterns and structure of the existing Next.js web application for a mobile-native context, leveraging a similar file-based routing paradigm.

The goal is to create a performant, maintainable, and scalable mobile application that delivers a user experience consistent with the web version.

---

## 2. Core Principles & Technology Stack

- **Framework:** React Native with Expo (Managed Workflow)
- **Language:** TypeScript
- **Navigation:** Expo Router (File-Based)
- **UI Components:** React Native Core Components + a UI library like `react-native-paper` for consistency.
- **Styling:** StyleSheet API
- **State Management:** Zustand (the existing stores can be reused with minimal changes).
- **Data Fetching:** `fetch` API via the existing `DataService`.

---

## 3. Recommended Project Structure (with Expo Router)

This structure aligns with Expo Router's conventions and separates concerns logically.

```
.
├── app/
│   ├── (tabs)/
│   │   ├── home/
│   │   │   ├── _layout.tsx      // Stack navigator for the home tab
│   │   │   ├── index.tsx        // HomeScreen
│   │   │   ├── shops-list.tsx
│   │   │   └── shop/
│   │   │       ├── [name].tsx
│   │   │       └── [category].tsx
│   │   ├── orders.tsx
│   │   └── profile.tsx
│   ├── _layout.tsx              // Root layout (handles tabs vs auth stack)
│   ├── auth/
│   │   ├── _layout.tsx          // Stack navigator for auth flow
│   │   ├── login.tsx
│   │   └── register.tsx
│   └── checkout.tsx             // Presented modally
├── assets/
│   ├── fonts/
│   └── images/
├── src/
│   ├── api/
│   │   └── dataService.ts       // (Can be reused from web)
│   ├── components/
│   │   ├── shared/              // Reusable, generic components
│   │   ├── ui/                  // UI library components
│   │   └── ...
│   ├── store/
│   │   ├── authStore.ts         // (Can be reused)
│   │   ├── cartStore.ts         // (Can be reused)
│   │   └── ...
│   ├── theme/
│   │   └── theme.ts
│   └── utils/
│       └── ...
└── package.json
```

---

## 4. Navigation (Expo Router)

Expo Router allows us to define our navigation declaratively using files and directories, similar to Next.js.

1.  **Root Layout (`app/_layout.tsx`):** This is the entry point. It will contain a top-level Stack navigator that conditionally shows either the `(tabs)` layout or the `(auth)` layout based on the user's login state from `authStore`.
2.  **Auth Stack (`app/auth/_layout.tsx`):** A stack for the pre-login flow. The `index.tsx` file inside this directory will be the default screen (e.g., could redirect to `login.tsx`). `login.tsx` and `register.tsx` will be the screens in this stack.
3.  **Main Tabs (`app/(tabs)/_layout.tsx`):** This file defines the main bottom tab navigator for the core app experience after login. It will define three tabs: `home`, `orders`, and `profile`.
    -   **Home Tab Stack (`app/(tabs)/home/_layout.tsx`):** The `home` tab is a nested stack navigator itself, allowing users to navigate from the `index.tsx` (the main home screen) to the `shops-list.tsx`, `shop/[name].tsx`, etc., all within the Home tab.
4.  **Modal Screens (`app/checkout.tsx`):** By defining this screen at the root level and configuring it in the root `_layout.tsx` as a modal, it can be presented over the entire application for a focused checkout experience.

**Example Libraries:**
`expo-router`, `expo-linking`, `react-native-screens`, `react-native-safe-area-context`

---

## 5. Component Translation Strategy

The web components can be mapped to React Native components. A UI library like `react-native-paper` can accelerate this process and provide a consistent Material Design look.

| Web Component (`shadcn/ui`) | React Native Equivalent (Core or `react-native-paper`) | Notes |
| ----------------------------- | -------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------- |
| `<Card>` | `Card` from `react-native-paper` | Provides `Card.Title`, `Card.Content`, `Card.Cover` which map well. |
| `<Button>` | `Button` from `react-native-paper` | Offers different modes (`contained`, `outlined`) that match the web variants. |
| `<Input>` | `TextInput` from `react-native-paper` | Includes label handling, error states, and theming. |
| `<Dialog>` / `<Sheet>` | `Modal` or `Portal` + `Dialog` from `react-native-paper` | `Modal` is best for full-screen takeovers like the search or checkout confirmation. |
| `<Avatar>` | `Avatar.Text` from `react-native-paper` | Directly maps to the user initial avatar. |
| `<Carousel>` | `react-native-reanimated-carousel` or build with `FlatList` | A dedicated library is recommended for performance. `FlatList` with `horizontal={true}` and `pagingEnabled` is a good starting point. |
| `<FloatingCart>` | `FAB` (Floating Action Button) from `react-native-paper` | The `FAB` component is perfect for this. It can be placed inside a `Portal` to float above all other content. |
| `<NavBar>` | Expo Router's `Tabs` component | The navigator will handle the UI and state of the main app sections. Icons from `react-native-vector-icons/MaterialCommunityIcons` can be used. |
| `<Header>` | Custom Component (`AppHeader.tsx`) | Use `Appbar` from `react-native-paper` as a base to create a custom, reusable header for all screens in the stack navigators. |
| Layout (`div`, `flex`) | `View`, `ScrollView`, `FlatList`, `SafeAreaView` | `View` is the primary container. Use `FlatList` for long lists of data (products, orders) for performance. `SafeAreaView` is crucial for avoiding notches. |
| Text (`p`, `h1`) | `Text` from `react-native-paper` or React Native Core | `react-native-paper`'s `Text` component respects the theme and provides variants (`titleLarge`, `bodyMedium`, etc.). |

---

## 6. State Management & Data Fetching

- **Zustand Stores:** The existing stores (`authStore`, `cartStore`, `superSaverStore`) can be reused almost directly. The logic for managing state is framework-agnostic. The only change is how you persist the state. Instead of `localStorage`, you will use `@react-native-async-storage/async-storage`.

  ```typescript
  import AsyncStorage from '@react-native-async-storage/async-storage';
  import { createJSONStorage } from 'zustand/middleware';

  // In your store creation:
  persist(
    (set) => ({ ... }),
    {
      name: 'auth-storage',
      storage: createJSONStorage(() => AsyncStorage), // Use AsyncStorage for native
    }
  )
  ```

- **DataService:** The `dataService.ts` file can also be reused as-is, since it uses the universal `fetch` API. No changes are needed here until you replace it with a real backend.

---

## 7. Theming

Centralize all design tokens (colors, fonts, spacing) in a `src/theme/theme.ts` file. This file will export a theme object that can be passed to `PaperProvider` from `react-native-paper`. This ensures a consistent look and feel across the entire application and makes future design changes easy.

```typescript
// src/theme/theme.ts
import { MD3LightTheme as DefaultTheme } from 'react-native-paper';

export const theme = {
  ...DefaultTheme,
  colors: {
    ...DefaultTheme.colors,
    primary: '#008080', // Teal
    background: '#F0F0F0', // Light Gray
    accent: '#90EE90', // Light Green (or use 'secondary')
    // ... other color overrides
  },
};
```
```typescript
// app/_layout.tsx
import { PaperProvider } from 'react-native-paper';
import { theme } from '../src/theme/theme';
import { Stack } from 'expo-router';

export default function RootLayout() {
  return (
    <PaperProvider theme={theme}>
      <Stack>
        {/* ... your layout logic ... */}
      </Stack>
    </PaperProvider>
  );
}
```
