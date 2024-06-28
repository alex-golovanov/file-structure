# FE project file structure

## Entity file structure

This is the basic file structure that will manifest itself in all folders, whether it's a component, layout or anything in between.

```
├── Entity/                  # component folder is capitalized to match component name
│   ├── index.ts                # index file serves as a single point for exports
│   ├── Entity.ts(x)           # contains component code
│   ├── constants.ts            # string literals/numbers, config objects, etc.
│   ├── enums.ts                # enums only
│   ├── styles.module.css       # styles for current component
│   ├── types.ts                # contains common TS annotations
│   ├── components/             # recursion of the parent component structure
│   │    ├── ChildComponent/
│   │    ├── AnotherChildComponent/
│   │    └── index.ts
│   ├── utils/
│   │    ├── awesomeUtil.ts
│   │    ├── usefulHelper.ts
│   │    └── index.ts           # used to export utils that will be imported outside this folder
│   └── hooks/
│        ├── awesomeHook.ts
│        └── index.ts           # used to export utils that will be imported outside this folder
└── ...
```

`Entity/index.ts` -> the entry point it defines clear boundaries as to what can be imported from this entity. All of the exports from this entry must be explicit.

```ts
// main entity should be exported as default for lazy loading
export { Entity as default } from "./Entity.tsx";
export { ENTITY_CONSTANT_FOR_EXTERNAL_EXPORT } from "./constants";
export type { EntityProps } from "./types";
```

#### ↓ `Entity/Entity.ts(x)` ↓

> Entity's code, this can be a component, class or anything else really.

```tsx
// ↓↓↓ `Entity/Entity.ts(x)` -> entity's code, this can be a component, class or anything else really.

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

#### ↓ `Entity/constants.ts` ↓

> Note that only certain constants need to be exported out of the entity index, hence it's important to be explicit and refrain from exporting everything.

```ts
export const ENTITY_CONSTANT_FOR_EXTERNAL_EXPORT = 42;
export const INTERNAL_ENTITY_CONSTANT_FOR_EXTERNAL_EXPORT = 37;
```

#### ↓ `Entity/enums.ts` ↓

> While they could be placed with constants, enums get their own file due to their unique property of being value and type at the same time.

```ts
export enum EntityStatus {
  ACTIVE = "active",
  INACTIVE = "inactive",
  DELETED = "deleted",
}
```

#### ↓ `Entity/styles.module.css` ↓

> At this point usage of CSS modules as default solution is undecided.

> For consistency it's advised to call upper level class - `root`.

> Exporting styles outside of entity is not advised.

```css
.root {
  /* CSS rules */
}
```

#### ↓ `Entity/types.ts` ↓

> All related type annotations, there is no discrimination between types and interfaces.

```ts
export interface EntityProps {
  //...
}

export type EntityState = {
  // ...
};
```

#### ↓ `Entity/utils/index.ts` ↓

> One utility per file. Utils may be moved in

```ts
// Single point of export for utility functions
export * from "./awesomeUtil";
export * from "./usefulHelper";
```

#### ↓ `Entity/components/index.ts` ↓

> All entities in Entity/components folder repeat current structure exposing exports through index file. Usually most of the things declared at lower are created for local consumption.

```ts
export { ChildComponent } from "./ChildComponent";
export { AnotherChildComponent } from "./AnotherChildComponent";
```

Brief description:

- `index.ts`: The entry point of the component, where you can export the component's functionality.
- `constants.ts`: A file to store any constants or configuration specific to the component.
- `types.ts`: A file to define any custom types or interfaces used by the component.
- `styles.css`: A CSS modules file to define the component's styles.
- `Component.tsx`: React component itself, file name is the same as display name.
- `utils/`: Pure function helpers.
- `hooks/`: Common state helpers, can also be used to extract state to improve readability and ease testing.
- `components/`: Child components, each component repeats the structure documented above (fractal).

Common patterns:

- the key idea behind this structure is modularity, by keeping all concerns inside of a module it becomes atomic/portable.
- all files should be named in `camelCase` convention, exceptions are components and classes, they follow `PascalCase` convention.
- Component should have clearly defined boundaries. Things (constants, utils, hooks, types, etc.) that are intended to be used outside of the component need to be explicitly exported from root `index` file. Same goes for internal component imports. In general, all imports must be done from `index` file, that way we can be certain that we import things that are designed for external consumption. To reiterate, `index` file should almost always be the entry point.
