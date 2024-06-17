# FE project file structure

## Component File Structure

To create a component file structure that includes `index.ts`, `constants.ts`, `types.ts`, and some common front-end files, you can follow this directory structure:

```
├── Component/                  # component folder is capitalized to match component name
│   ├── index.ts                # index file serves as a single point for exports
│   ├── Component.tsx           # contains component code
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

- all files should be named in `camelCase` convention, exceptions are components and classes, they follow `PascalCase` convention.
- Component should have clearly defined boundaries. Things (constants, utils, hooks, types, etc.) that are intended to be used outside of the component need to be explicitly exported from root index file. Same goes for internal component imports. In general, all imports must be done from index file, that way we can be certain that we import things that are designed for external consumption.
