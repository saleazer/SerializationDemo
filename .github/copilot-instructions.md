## Code
- No semicolons
- No publicly available code
- Next.js 14 Pages Router only
- No class components; no `any`

## Stack
Sitecore XM Cloud, JSS 21.x, Next.js 14, React 18, TypeScript strict, Apollo Client, Redux Toolkit, react-hook-form, SASS modules, FontAwesome

## Structure (`src/Rendering/src/`)
- `components/Name/` — component folder; files: `Name.tsx`, `Name.spec.tsx`, `NameDefinitions.ts`, `styles.module.scss`, `types.ts`, variant sub-components
- `renderings/` — thin re-export wrappers for Sitecore
- `helpers/`, `hooks/`, `contexts/`, `models/`, `lib/`, `services/`, `utilities/`, `types/`, `layouts/`, `pages/`

## Path Aliases (never use relative `../../` paths)
`components/*`, `hooks/*`, `helpers/*`, `contexts/*`, `models/*`, `lib/*`, `pages/*`, `types/*`, `loves-shared/*` → `src/utilities/*`, `@/*` → `src/*`

## Components
- Named export matching folder: `export const Name = ...`
- `src/renderings/Name.tsx`: `import { Name } from 'components/Name'; export default Name`
- Datasource-dependent components: wrap with `withDatasourceCheck` from `@sitecore-jss/sitecore-jss-nextjs`
- Multiple visual modes: variants map keyed by name, resolved from `props.params.fieldNames`
- Field types/props in `NameDefinitions.ts` using JSS types: `Field<string>`, `ImageField`, `LinkField`

## Redux
- Slices live in `services/` (not `store/` or `features/`)
- Always handle `HYDRATE` from `next-redux-wrapper` in `extraReducers`
- Export selectors via `createSelector`; export actions separately
- Pages using Redux SSR: `wrapper.getServerSideProps(store => ...)` or `wrapper.withRedux(Page)`

## GraphQL
- Import `gql` from `graphql-tag`, not `@apollo/client`
- Queries in a separate `GetNameQuery.ts` file co-located with the component
- Use inline fragments (`... on TypeName`) for Sitecore template fields

## Environment / Config
- Never use `process.env` directly in client components — use `useRuntimeConfig()` from `hooks/useRuntimeConfig`
- `useRuntimeConfig()` reads `window.__RUNTIME_CONFIG__` on client, `process.env` on server

## Testing
- Cypress component testing only — not Jest, not Vitest
- Test file: same directory and extension as source, `.spec.` suffix (e.g. `Hero.tsx` → `Hero.spec.tsx`)
- Use `cy.mount()`; run with `npm run test` or `npm run test:ci`