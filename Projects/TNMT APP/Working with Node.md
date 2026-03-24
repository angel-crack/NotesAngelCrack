```bash
docker run -it --rm \
  -v $(pwd):/app \
  -w /app \
  node:latest \
  bash
```

Installing VITE

```bash
root@26f7228a4446:/app# npm create vite@latest
Need to install the following packages:
create-vite@8.3.0
Ok to proceed? (y) 

> npx
> "create-vite"

│
◇  Project name:
│  frontend
│
◇  Select a framework:
│  React
│
◇  Select a variant:
│  TypeScript
│
◇  Use Vite 8 beta 
│  (Experimental)?:
│  No
│
◇  Install with npm and start now?
│  Yes
│
◇  Scaffolding project in /app/frontend...
│
◇  Installing dependencies with npm...

added 175 packages, and audited 176 packages in 22s

45 packages are looking for funding
  run `npm fund` for details

found 0 vulnerabilities
│
◇  Starting dev server...

> frontend@0.0.0 dev
> vite


  VITE v7.3.1  ready in 198 ms

  ➜  Local:   http://localhost:5173/
  ➜  Network: use --host to expose
  ➜  press h + enter to show help

```

Now it is running