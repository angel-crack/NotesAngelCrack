[+] Cuando agregamos los recursos el elemento app-root está haciendo referencia al componente padre padre

```powershell
project-a/
┣ .angular/
┣ .vscode/
┣ node_modules/
┣ src/
	┣ app/
	┣ assets/
	┣ favicon.ico
	┣ index.html
```

```html info:15
			<!doctype html>
			<html lang="en">
			<head>
			  <meta charset="utf-8">
			  <title>ProjectA</title>
			  <base href="/">
			  <meta name="viewport" content="width=device-width, initial-scale=1">
			  <link rel="icon" type="image/x-icon" href="favicon.ico">
		  	  <link rel="stylesheet" href="https://unicons.iconscout.com/release/v4.0.8/css/line.css">
		      <link rel="preconnect" href="https://fonts.googleapis.com">
		      <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
			  <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@400;500;600&display=swap" rel="stylesheet">
			</head>
			<body>
				<app-root></app-root>
			</body>
			</html>
```

[+] Este componente padre padre lo vamos a encontrar dentro de app

```powershell info:7
app/
┣ core/ 
┣ data/
┣ modules/
┣ shared/ 
┣ app-routing.module.ts
┣ app.component.css ⇒ Los estilos que aplican solo al archivo de abajo
```

```css
	h1 {
	    background-color: red;
	    width: 20%;
	}
```

```powershell info:2

┣ app.component.html ⇒ El codigo HTML del elemento
```
```html
	<h1>Main App component of {{title}} </h1>
	<router-outlet></router-outlet>
```
```powershell
┣ app.component.spec.ts
┣ app.component.ts
```
```ts
	import { Component } from '@angular/core';

	@Component({ ⇒ Decorador que importamos arriba
	  selector: 'app-root', ⇒ Asi lo vamos a referenciar en el HTML
	  templateUrl: './app.component.html', ⇒ Ruta del HTML del componente
	  styleUrls: ['./app.component.css'] ⇒ Ruta del CSS del componente
	})
	export class AppComponent { ⇒ Aqui vamos a exportar la clase
	  title = 'project-a'; ⇒ Propiedades del componente, notese que el HTML podemos usarlo con doble llave {{ _PROPERTY_ }}
	}
```
```powershell
┗ app.module.ts
```

[+] Compilado se ve así:

![[renderapproot.png]]
