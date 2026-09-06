## Commands

Client (`MERN_Boilerplate/client`):
- `npm run dev` — Vite dev server
- `npm run build` — Vite production build
- `npm run lint` — `eslint .`
- `npm run preview` — preview production build

Server (`MERN_Boilerplate/server`):
- `npm run dev` — `nodemon index.js`
- `npm start` — `node index.js`
- `npm test` — not configured (`echo "Error: no test specified" && exit 1`)

## Rules
- Use npm, not pnpm/yarn — `package-lock.json` present in both `client/` and `server/`, no other lockfile observed.
- `client/package.json` and `server/package.json` both set `"type": "module"` — use ESM `import`/`export`, not `require`.

## Read first
- README.md
- MERN_Boilerplate/server/index.js
- MERN_Boilerplate/client/src/main.jsx

Architecture: see ARCHITECTURE.md — read before structural changes
