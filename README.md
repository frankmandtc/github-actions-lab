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