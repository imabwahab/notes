# Node.js Project Setup with TypeScript

This guide sets up a basic Node.js project using TypeScript.

## 1. Create the project

```bash
mkdir my-node-ts-app
cd my-node-ts-app
npm init -y
```

## 2. Install dependencies

Install TypeScript and Node.js type definitions:

```bash
npm install -D typescript ts-node @types/node
```

Optional but useful for auto-reload during development:

```bash
npm install -D nodemon
```

## 3. Initialize TypeScript

Create a `tsconfig.json` file:

```bash
npx tsc --init
```

Replace it with:

```json
{
  "compilerOptions": {
    "target": "ES2020",
    "module": "CommonJS",
    "rootDir": "./src",
    "outDir": "./dist",
    "strict": true,
    "esModuleInterop": true,
    "skipLibCheck": true,
    "forceConsistentCasingInFileNames": true
  },
  "include": ["src"],
  "exclude": ["node_modules", "dist"]
}
```

## 4. Update `package.json` scripts

Add these scripts:

```json
{
  "scripts": {
    "dev": "nodemon --watch src --ext ts --exec ts-node src/index.ts",
    "build": "tsc",
    "start": "node dist/index.js"
  }
}
```

If you do not want `nodemon`, use:

```json
{
  "scripts": {
    "dev": "ts-node src/index.ts",
    "build": "tsc",
    "start": "node dist/index.js"
  }
}
```

## 5. Create the folder structure

```bash
mkdir src
```

Project structure:

```text
my-node-ts-app/
|-- src/
|   `-- index.ts
|-- dist/
|-- package.json
|-- tsconfig.json
`-- node_modules/
```

## 6. Add starter code

Create `src/index.ts`:

```ts
const message: string = "Hello from Node.js with TypeScript";

console.log(message);
```

## 7. Run the project

For development:

```bash
npm run dev
```

Build the project:

```bash
npm run build
```

Run compiled JavaScript:

```bash
npm start
```

## 8. Common setup summary

Commands in one place:

```bash
npm init -y
npm install -D typescript ts-node @types/node nodemon
npx tsc --init
mkdir src
```

## 9. Optional `.gitignore`

Create a `.gitignore` file:

```gitignore
node_modules
dist
.env
```

## 10. Recommended next additions

- Add ESLint and Prettier
- Add environment variable support with `dotenv`
- Add testing with `vitest` or `jest`
- Add path aliases if the project grows
