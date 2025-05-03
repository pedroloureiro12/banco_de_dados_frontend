#  Roteiro para a utilização do projeto

# 1 passo:
a instalação Node.js e NPM, entre no site do Node.js (https://nodejs.org/en) e baixe a versão recomendada (LTS) e siga o comando a baixo para realizar de forma correta

```bash
node -v
```
Se o node tiver a versão 22.9.0 é a correta. agora verificamos o gerenciador de pacotes npm com o comando baixo:
```bash
npm -v
```
Isso irá mostrar a versão do Node e NPM caso esteja instalado corretamente.

2° passo: Instalação e configuração do Quasar Framework

para a realização do projeto, é necessário instalar o Quasar CLI pelo seguinte comando

```bash
npm install -g @quasar/cli 
``` 

Para depois iniciamos o projeto com o comando no terminal:

```bash
npm init quasar@latest
````
3° Passo: Escolha de builds e configurações do projeto

Para conseguirmos executar o código, necessita de algumas definições escolhidas quando se inicia o projeto, que serão as 

```
APP with Quasar CLI, let's go!
Quasar APP CLI with Vite
CRUD simples
Composition API with <script setup>
Nome(the others Will still be avalable)
Linting( vite-plugin-checker + ESLint) vue-118n
yes, use npm
```
Para conseguimos modificar as nossas configurações definidas, aplicamos o seguintes comandos

```bash
cd nome_da_pasta
code .
``` 
4° passo: Criação do projeto CRUD

Para se criar o CRUD, será necessário algumas pequenas modificações e criações de arquivos e pastas, entre o primeiro será a remoção do 'EssentialLink.vue'. 

A remoção da linha de código do arquivo 'IndexPage.vue' que será:

```javascript
<img 
   alt= "Quasar logo"
   src = "~assets/quasar-logo-vertical.svg"
   styler =



