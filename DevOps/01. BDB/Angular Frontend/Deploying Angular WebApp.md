[+] Once new task it is created and it is stored on github, we proceed to clone it under Projects, it will create a folder with the project:

```c
Projects/
├─ new_BDB_Project/
```

[+] Open the folder "new_BDB_Project", then deploy a new angular project

```bash
ng new frontend
? Would you like to add Angular routing? Yes
? Which stylesheet format would you like to use? CSS
```

```c
Projects/
├─ new_BDB_Project/
│  ├─ frontend/
│  ├─ *.allBDBFiles
```

[+] Then, start the development of the application on angular.

[+] Once it is finished, run (your current working directory should be Projects/new_BDB_Project/frontend)

```bash
 ng build --base-href=./
```

[+] Commit the dist files to repository so they load in BDB app

```bash
cp -r dist/frontend/browser/* ../
```

## Commit Changes:

[+] On BDB Working Directory (Projects/new_BDB_Project/)

```bash
rm -f main.*.js 
rm -f polyfills.*.js 
rm -f runtime.*.js
rm -f styles.*.css
cp -r frontend/dist/frontend/browser/* .
git add *
git commit -a
git push
```

