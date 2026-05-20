# 1.- Workflow CI para el proyecto de frontend
Vamos a crear un workflow en Github para el proyecto hangman-front, donde vamos a usar CI para automatizar el build y los test unitarios del proyecto cuando hay cambios (push o pull requests) en la rama main.
La configuracioón del workflow sería el guardado en ci.yml, que se encuentra dentro de la carpeta .github/workflows (está en el repo).
El código quedaría así:
```
name: Integración Continua

on:
  push:
    branch: [ main ]
    paths: [ 'hangman-front/**']
  pull_request:
    branches: [ main ]
    paths: [ 'hangman-front/**']

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout
        uses: actions/checkout@v6
      - name: Set up Node.js version
        uses: actions/setup-node@v6
        with:
          node-version: 18
      - name: Build
        working-directory: ./hangman-front
        run: |
          npm ci
          npm run build
  
  test:
    runs-on: ubuntu-latest
    needs: build

    steps:
      - name: Checkout
        uses: actions/checkout@v6
      - name: Set up Node.js version
        uses: actions/setup-node@v6
        with:
          node-version: 18
      - name: Unit tests
        working-directory: ./hangman-front
        run: |
          npm ci
          npm run test
```


Vamos a definir los eventos de nuestro workflow: push y pull_request. Esto indica que cuando se produzca alguno de estos 2 eventos en nuestro github, lanzaremos los trabajos (jobs)

La primera prueba (haciendo un push) no me funciona (no se ejecuta el workflow). Recordando la clase on line del otro día, me doy cuenta que necesito cambiar algo del código fuente para que se lance.
![alt text](image.png)
Pero veo que se produce un error en el trabajo "test"
![alt text](image-1.png)
**Afortunadamente**, este mismo fallo le surgió a un compañero, y me indica el lugar en el que hay que realizar una modificación ( fichero *start-game.spec-tsx*)
Hago el cambio que me comenta y actualizo el repo en github
![alt text](image-2.png)
Y compruebo que esta vez se ha ejecutado el Workflow Integración Continua de forma satisfactoria
![alt text](image-3.png)
![alt text](image-4.png)
Compruebo el trabajo **"build"** y todos los pasos que se han realizado
![alt text](image-5.png)
Igual ocurre con el **"test"**
![alt text](image-6.png)

