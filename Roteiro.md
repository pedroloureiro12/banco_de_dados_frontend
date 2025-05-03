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
   styler = "with: 200px; height: 200px"
>
```
5°passo: Local Storage para guardar os dados

Criamos uma pasta e um arquivo no diretório src/utils sendo o arquivo com o nome utils.js, sendo nesse arquivo o seguinte código

```javascript
// Salva um value no localStorage com a chave key
function saveLocalStorage(key, value) {
    localStorage.setItem(key, JSON.stringify(value))
}

// Retorna o value salvo no localStorage com a chave key ou um array vazio caso não exista
function getLocalStorage(key) {
    return JSON.parse(localStorage.getItem(key)) || []
}

function addNewData(key, data) {
    // Busca os dados salvos no localStorage com a chave key
    const dataStorage = getLocalStorage(key)
    // Gera um id para o novo dado
    const id = dataStorage.length ? dataStorage[dataStorage.length - 1].id + 1 : 1
    // Cria um novo dado com o id gerado e os dados passados
    const newData = { id, ...data }
    // Adiciona o novo dado ao array de dados
    dataStorage.push(newData)
    // Salva o array de dados atualizado no localStorage
    saveLocalStorage(key, dataStorage)
}

function editData(key, id, data) {
    // Busca os dados salvos no localStorage com a chave key
    const dataStorage = getLocalStorage(key)
    // Busca o índice do dado que será editado
    const index = dataStorage.findIndex((item) => item.id === id)
    // Substitui o dado antigo pelo novo dado
    dataStorage[index] = { id, ...data }
    // Salva o array de dados atualizado no localStorage
    saveLocalStorage(key, dataStorage)
}

function deleteData(key, id) {
    // Busca os dados salvos no localStorage com a chave key
    const dataStorage = getLocalStorage(key)
    // Busca o índice do dado que será deletado
    const index = dataStorage.findIndex((item) => item.id === id)
    // Remove o dado do array
    dataStorage.splice(index, 1)
    // Salva o array de dados atualizado no localStorage
    saveLocalStorage(key, dataStorage)
}

export { savelocalStorage, getLocalStorage, addNewData, editData, deleteData}
```

6° passo:FormComponent.vue

O FormComponent.vue vai ser criado no src/components, sendo o responsável por ler e enviar os dados do usuário, o seu código é 

```vue




