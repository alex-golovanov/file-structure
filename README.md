# FE project file structure

## Entity file structure

This is the basic file structure that will manifest itself in all folders, whether it's a component, layout or anything in between.

```
├─> Entity
│   ├── index.ts
│   ├── Entity.ts(x)
│   ├── constants.ts
│   ├── enums.ts
│   ├── styles.module.css
│   ├── types.ts
│   ├─> components
│   │    ├─> ChildComponent
│   │    ├─> AnotherChildComponent
│   │    └── index.ts
│   ├─> utils
│   │    ├── awesomeUtil.ts
│   │    ├── usefulHelper.ts
│   │    └── index.ts
│   └─> hooks
│        ├── awesomeHook.ts
│        └── index.ts
└── ...
```

### Brief description:

- `index.ts`: The entry point of the component, where you can export the component's functionality.
- `constants.ts`: A file to store any constants or configuration specific to the entity.
- `types.ts`: Contains all relevant type annotations.
- `styles.module.css`: CSS modules file to define the entity's styles.
- `Entity.ts(x)`: Component/class itself.
- `utils/`: Pure function helpers.
- `hooks/`: Common state helpers, can also be used to extract state to improve readability and ease testing.
- `components/`: Child components, each component repeats the structure documented above (fractal).

### Detailed description

#### ↓ `index.ts` ↓

> The entry point it defines clear boundaries as to what can be imported from this entity. All of the exports from this entry must be explicit.

```ts
// main entity should be exported as default for lazy loading
export { Entity as default } from "./Entity.tsx";
export { ENTITY_CONSTANT_FOR_EXTERNAL_EXPORT } from "./constants";
export type { EntityProps } from "./types";
```

#### ↓ `Entity.ts(x)` ↓

> Entity's code, this can be a component, class or anything else really.

```tsx
// ↓↓↓ `Entity.ts(x)` -> entity's code, this can be a component, class or anything else really.

import React from "react";
import styles from "./styles.module.css";

interface EntityProps {
  // types required for entity are always defined immediately before the entity
}

const Entity: React.FC<EntityProps> = (props) => {
  return <div className={styles.root}>{/* Component content */}</div>;
};

// only named exports are allowed
export { Entity };
```

#### ↓ `constants.ts` ↓

> Constants are to be named in `CONSTANT_CASE` (also known as `SCREAMING_SNAKE_CASE`).
> Note that only certain constants need to be exported out of the entity index, hence it's important to be explicit and refrain from exporting everything.

```ts
export const ENTITY_CONSTANT_FOR_EXTERNAL_EXPORT = 42;
export const INTERNAL_ENTITY_CONSTANT_FOR_EXTERNAL_EXPORT = 37;
```

#### ↓ `enums.ts` ↓

> Enum _keys_ are to be named in `CONSTANT_CASE` (values case does not matter).
> While they could be placed with constants, enums get their own file due to their unique property of being value and type at the same time.

```ts
export enum EntityStatus {
  ACTIVE = "active",
  INACTIVE = "inactive",
  DELETED = "deleted",
}
```

#### ↓ `styles.module.css` ↓

> At this point usage of CSS modules as default solution is undecided.

> For consistency it's advised to call upper level class - `root`.

> Exporting styles outside of entity is not advised.

```css
.root {
  /* CSS rules */
}
```

#### ↓ `types.ts` ↓

> All related type annotations, there is no discrimination between types and interfaces.

```ts
export interface EntityProps {
  //...
}

export type EntityState = {
  // ...
};
```

#### ↓ `components/index.ts` ↓

> All entities in Entity/components folder repeat current structure exposing exports through index file. Usually most of the things declared at lower are created for local consumption.

```ts
export { ChildComponent } from "./ChildComponent";
export { AnotherChildComponent } from "./AnotherChildComponent";
```

#### ↓ `utils` ↓

> One utility/function per file.
> Explicit exports only, using \* is not allowed.
> The same structure applies to hooks - they are utilities for state extraction.

```ts
// awesomeUtil.ts
// One utility/function per file.
export const awesomeUtil = () => {
  // utility logic
};
```

```ts
// usefulHelper.ts
// One utility/function per file.
export const usefulHelper = () => {
  // helper logic
};
```

```ts
// index.ts
// Explicit exports only, using * is not allowed.
export { awesomeUtil } from "./awesomeUtil";
export { usefulHelper } from "./usefulHelper";
```

### Common patterns:

- The key idea behind this structure is modularity, by keeping all concerns inside of a module it becomes atomic/portable.
- All files should be named in `camelCase` convention, exceptions are components and classes, they follow `PascalCase` convention.
- Component should have clearly defined boundaries. Things (constants, utils, hooks, types, etc.) that are intended to be used outside of the component need to be explicitly exported from root `index` file. Same goes for internal component imports. In general, all imports must be done from `index` file, that way we can be certain that we import things that are designed for external consumption. To reiterate, `index` file should almost always be the entry point.

npx dree parse ./src

## Project file structure

> This is an example for project file structure. It adopts entity design described above.

```
src
 ├─> components
 │   ├─> Button
 │   │   ├── Button.tsx
 │   │   ├─> hooks
 │   │   │   ├── index.ts
 │   │   │   └── useButtonState.ts
 │   │   ├── index.ts
 │   │   ├── styles.module.css
 │   │   ├── types.ts
 │   │   └─> utils
 │   │       ├── index.ts
 │   │       └── resolveButtonMode.ts
 │   └── index.ts
 ├─> constants
 │   ├── global-domain-constants.ts
 │   └── index.ts
 ├─> enums
 │   ├── global-domain-enums.ts
 │   └── index.ts
 ├─> hooks
 │   ├── index.ts
 │   ├── useDebounce.ts
 │   ├── useEventCallback.ts
 │   └── useLazyRef.ts
 ├── index.tsx
 ├─> layouts
 │   ├─> IDELayout
 │   │   ├── IDELayout.tsx
 │   │   ├─> hooks
 │   │   ├── index.ts
 │   │   ├── styles.module.css
 │   │   └─> utils
 │   └── index.ts
 ├─> pages
 ├─> redux
 │   ├── constants.ts
 │   ├── index.ts
 │   ├─> slices
 │   │   ├── index.ts
 │   │   └─> slice-or-feature
 │   │       ├── actions.ts
 │   │       ├── index.ts
 │   │       ├── reducer.ts
 │   │       └── sagas.ts
 │   ├── store.ts
 │   └─> utils
 ├─> styles
 │   ├── global.css
 │   ├── theme-dark.module.css
 │   └── theme-light.module.css
 ├─> types
 │   ├── global-domain-types.ts
 │   └── index.ts
 └─> utils
     ├─> date
     │   ├── index.ts
     │   └── toLocalTime.ts
     ├─> etc
     │   ├── etc.ts
     │   └── index.ts
     ├── index.ts
     ├─> string
     │   ├── index.ts
     │   └── toCamelCase.ts
     └─> validation
         ├── index.ts
         └── isValidEmail.ts
```

### Brief description of an example structure

**`components`**: This directory contains reusable React components that can be used throughout the application.

- **`Button`**: An example component folder.
  - **`Button.tsx`**: The main file for the Button component.
  - **`hooks`**: Contains custom hooks used specifically within the Button component.
    - **`index.ts`**: Aggregates exports of all hooks in this folder.
    - **`useButtonState.ts`**: A custom hook for managing button state.
  - **`index.ts`**: Entry point for the Button component, exporting everything related to it.
  - **`styles.module.css`**: CSS module for styling the Button component.
  - **`types.ts`**: TypeScript types and interfaces used by the Button component.
  - **`utils`**: Utility functions used specifically within the Button component.
    - **`index.ts`**: Aggregates exports of all utility functions in this folder.
    - **`resolveButtonMode.ts`**: A utility function for resolving the button mode.
- **`index.ts`**: Entry point for the components directory, exporting all components.

**`constants`**: Stores constant values used throughout the application.

- **`global-domain-constants.ts`**: Global constants.
- **`index.ts`**: Aggregates and exports all constants.

**`enums`**: Contains enum definitions used throughout the application.

- **`global-domain-enums.ts`**: Global enums.
- **`index.ts`**: Aggregates and exports all enums.

**`hooks`**: Contains reusable custom hooks that can be used throughout the application.

- **`index.ts`**: Aggregates and exports all hooks.
- **`useDebounce.ts`**: Custom hook for debouncing.
- **`useEventCallback.ts`**: Custom hook for event callbacks.
- **`useLazyRef.ts`**: Custom hook for lazy refs.

**`index.tsx`**: The main entry point of the application. Typically includes the root component and rendering logic.

**`layouts`**: Contains layout components, which are used to define the structure of pages or sections of the application.

- **`IDELayout`**: An example layout component.
  - **`IDELayout.tsx`**: The main file for the IDE layout component.
  - **`hooks`**: Custom hooks used within the IDE layout component.
  - **`index.ts`**: Aggregates and exports everything related to the IDE layout component.
  - **`styles.module.css`**: CSS module for styling the IDE layout component.
  - **`utils`**: Utility functions used within the IDE layout component.
- **`index.ts`**: Aggregates and exports all layout components.

**`pages`**: Contains the different page components for the application. Each file or folder in this directory represents a different route or page.

**`redux`**: Contains Redux-related files for state management.

- **`constants.ts`**: Redux-related constants.
- **`index.ts`**: Aggregates and exports all Redux-related files.
- **`slices`**: Directory for Redux slices, where each slice represents a feature of the Redux store.
  - **`index.ts`**: Aggregates and exports all slices.
  - **`slice-or-feature`**: A specific slice or feature.
    - **`actions.ts`**: Redux actions for this slice.
    - **`index.ts`**: Aggregates and exports everything related to this slice.
    - **`reducer.ts`**: Redux reducer for this slice.
    - **`sagas.ts`**: Redux sagas for this slice.
- **`store.ts`**: Configuration of the Redux store.
- **`utils`**: Utility functions related to Redux.

**`styles`**: Contains global and theme-specific styles for the application. - **`global.css`**: Global CSS styles. - **`theme-dark.module.css`**: CSS module for the dark theme. - **`theme-light.module.css`**: CSS module for the light theme.

**`types`**: Contains TypeScript type definitions and interfaces used throughout the application. - **`global-domain-types.ts`**: Global types. - **`index.ts`**: Aggregates and exports all type definitions.

**`utils`**: Contains utility functions and helpers used throughout the application. - **`date`**: Directory for date-related utility functions. - **`etc`**: Just a placeholder to depict that there can be other areas. - **`index.ts`**: Aggregates and exports all utilities. - **`string`**: Directory for string-related utility functions. - **`validation`**: Directory for validation-related utility functions.
