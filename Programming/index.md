---
slug: build_react
title: Build React Apps
authors: [aubarnes]
tags: [env, backend, build]
---

This documentation outlines the structure and setup process of a Vite + React project intended for integration with a BDB (Business Data Backend) environment. It includes project initialization, custom build steps, and environment-specific configurations to ensure seamless deployment and execution.

---
<!-- truncate -->

## 📁 Initial Project Structure

The starting structure of the project is as follows:

```powershell
DefectiveRMAReport/  
┣ modules/  
┣ my-app/  
┃ ┣ .vite/  
┃ ┣ node_modules/  
┃ ┣ public/  
┃ ┣ src/  
┃ ┣ .
┃ ┣ package.json
┃ ┣ .
┃ ┗ vite.config.ts  
┣ .gitignore  
┣ bdb.json  
┗ __init__.py
```

## 📦 `package.json` Overview

A simplified version of the default `package.json`:

```json
{
  "name": "reacttest",
  "private": true,
  "version": "0.0.0",
  "type": "module",
  "scripts": {
    "dev": "vite",
    "build": "tsc -b && vite build",
    "lint": "eslint .",
    "preview": "vite preview"
  },
}

```
## ✅ Custom Scripts (Final Version)

These scripts replace the default ones:

```json
"scripts": {
    "create-envs": "powershell -Command \"New-Item ./.env.development -ItemType File -Force | Out-Null; New-Item ./.env.production -ItemType File -Force | Out-Null\"",
    "start": "vite",
    "dev": "vite --mode development",
    "prod": "vite --mode production",
    "prebuild": "powershell -ExecutionPolicy Bypass -File ../clean-root-build.ps1",
    "build": "tsc -b && vite build --mode production",
    "postbuild": "powershell Copy-Item ../dist/* ../ -Recurse -Force",
    "lint": "eslint .",
    "preview": "vite preview"
  }
```

Note: All these scripts will be accessible from `npm run <script-name>`

let's break down each one:

## 1️⃣ `npm run create-envs`

Creates two environment files at the root of the React app:

```powershell
DefectiveRMAReport/  
┣ modules/  
┣ my-app/  
┃ ┣ .vite/  
┃ ┣ node_modules/  
┃ ┣ public/  
┃ ┣ src/  
┃ ┣ .env.development ==>  This One
┃ ┣ .env.production ==> This One
┃ ┣ package.json
┃ ┣ .
┃ ┗ vite.config.ts  
┣ .gitignore  
┣ bdb.json  
┗ __init__.py
```

### 🔐 .env File Usage

- **Vite requires the `VITE_` prefix** for any environment variable that needs to be accessed in the frontend.
- All variables should be declared in **both** `.env.development` and `.env.production`.
## How to use .env files?

All the variables should be defined on both files, even if it is the same. And it is mandatory to use the VITE_ prefix (such as VITE_APP_) for environment variables you want to access in your Vite React app.

More:
https://vite.dev/guide/env-and-mode

**Why?**

- Vite only exposes environment variables that start with ``VITE_`` to your client-side code.
- Any variable not starting with ``VITE_`` will not be available in ``import.meta.env`` in your app.

**Example:**

- `VITE_APP_API_BASE_URL` is accessible as `import.meta.env.VITE_APP_API_BASE_URL`.
- `APP_API_BASE_URL` (without the prefix) will NOT be accessible in your frontend code.

**Summary:**  
Always use the VITE_ prefix for any environment variable you want to use in your Vite-powered frontend.

![prod_dev](./prod_dev.png)

For example, this is how we access across the whole project:

``` ts
const url = import.meta.env.VITE_APP_API_BASE_URL;
```

So, when we run the application in `development` mode it will use variables from file`.env.development`, and the same for `production`, it will use `.env.production` and so on. We will explain how to initiate the app with one mode or another.

# 2️⃣ `npm run dev` & `npm run prod`

These scripts start the app with the respective environment mode:
- `npm run dev` → uses `.env.development`
- `npm run prod` → uses `.env.production`

Internally equivalent to: `npm run start -- --mode <mode>`

# 3️⃣ Build Lifecycle Scripts

- When you run `npm run build`, npm will automatically run `prebuild` before `build`, and `postbuild` after `build`, if those scripts exist in your `package.json`.
- This is a built-in npm feature:
    - `prebuild` runs before `build`
    - `build` runs
    - `postbuild` runs after `build`

So, the sequence is:

1. `prebuild` (your clean script)
2. `build` (TypeScript + Vite build)
3. `postbuild` (copy files to root)

You do not need to call them manually—npm handles the order for you.  

This pattern works for any npm script: `pre<name>`, `<name>`, `post<name>`.

**Why 3 Steps?**

Typically, when you build the application over `npm run build` Vite will generate a `dist` folder under `my-app/` 

```powershell
DefectiveRMAReport/  
┣ modules/  
┣ my-app/  
┃ ┣ .vite/  
┃ ┣ node_modules/  
┃ ┣ public/  
┃ ┣ dist/ ==> FOLDER GENERATED  
┃ ┣ src/  
┃ ┣ .env.development
┃ ┣ .env.production
┃ ┣ package.json
┃ ┣ .
┃ ┗ vite.config.ts  
┣ .gitignore  
┣ bdb.json  
┗ __init__.py
```

However, we want the following structure

```powershell
DefectiveRMAReport/  
┣ modules/  
┣ dist/ ==> FOLDER GENERATED  
┣ my-app/  
┣ copy the content of Dist here, in order to expose index.html for BDB to run the WebApp
┣ .gitignore  
┣ bdb.json  
┗ __init__.py
```

But ``dist`` folder will be containing different files along the time, so, what are we going to do first is to check in our current folder, list elements and delete all of them at root. Then build the application and finally copy all inside to root.


### For Prebuild

Lets create a file under root called `clean-root-build.ps1` and the content will be 

```bash
$distPath = "../dist"
$rootPath = "../."

if (-not (Test-Path $distPath)) {
    Write-Host "No dist folder found, nothing to clean."
    exit 0
}

$distRoot = (Get-Item $distPath).FullName

Get-ChildItem -Path $distPath -Recurse | ForEach-Object {
    $relativePath = $_.FullName.Substring($distRoot.Length).TrimStart('\','/')
    $targetPath = Join-Path $rootPath $relativePath
    if (Test-Path $targetPath) {
    
        if ((Get-Item $targetPath).PSIsContainer) {
            Remove-Item $targetPath -Recurse -Force
        } else {
            Remove-Item $targetPath -Force
        }
    }
}
```

This file will be the responsible for list elements under `dist` and remove all of them at `/` 

### For Build 

Let's modify our `vite.config.ts` to be like:

```ts
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'

// https://vite.dev/config/
export default defineConfig({
  plugins: [react()],
  build: {
    outDir: '../dist',
    emptyOutDir: true,
  },
  base: './',
});
```

This will lead us to create `dist` folder at parent director, which is `/`, it will erase everything inside that folder. And finally indicate to use base reference for the HTML as `./`

So, instead of having the following output:

```html
    <link rel="icon" type="image/svg+xml" href="/report-svgrepo-com.svg" />
```

We will have 

```html
    <link rel="icon" type="image/svg+xml" href="./report-svgrepo-com.svg" />
```

Same for other references.

### For Post Build

When build finishes, this will be our structure:

```powershell
DefectiveRMAReport/    
┣ dist/  
┣ modules/  
┣ my-app/  
┣ .env.development  
┣ .env.production  
┣ .gitignore  
┣ bdb.json  
┣ clean-root-build.ps1    
┗ __init__.py
```

and the contnet of `dist`

```powershell
dist/  
┣ assets/  
┣ index.html  
┗ report-svgrepo-com.svg
```

This will be copying everything inside of `dist` to `/`, the final structure will be like:

```powershell
DefectiveRMAReport/  
┣ assets/  
┣ dist/  
┣ modules/  
┣ my-app/  
┣ .env.development  
┣ .env.production  
┣ .gitignore  
┣ bdb.json  
┣ clean-root-build.ps1  
┣ index.html  
┣ report-svgrepo-com.svg  
┗ __init__.py
```

 ⚠️ Pushing that to BDB will avoid us for extra steps in order to update our React App. This setup ensures BDB can access `index.html` and related assets directly from the root without needing to reference the `my-app` folder.

## ✅ Conclusion

This structure and lifecycle ensure:

- Easy mode switching (`development` / `production`)
- Clear environment management
- Clean and consistent deployment artifacts
- Seamless integration with systems expecting a root-level `index.html`

Deploying or updating the app in BDB becomes as simple as:

``` powershell
cd my-app/
npm run build
```


And you're done.